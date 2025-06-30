# enigma
python code for enfrypting massage using the enigma machine method from WW2

import string
class board:
    order='abcdefghijklmnopqrstuvwxyz'
   
    def letter_return(self, number):
        return self.order[number-1]
    
    def letter_change(self):
        chosen_pos1=int(input("enter first position: "))
        chosen_pos2=int(input("enter second position: "))
        temp="#"
        text=self.order.replace(self.order[chosen_pos1],temp).replace(self.order[chosen_pos2],self.order[chosen_pos1]).replace(temp,self.order[chosen_pos2])
        self.order=text

    def encode(self, text):
        code=list("a"*len(text))
        for i in range(len(text)):
            code[i]=self.letter_return(get_letter_position(text[i]))
        return code
 
class rotor(board):

    def encode(self, text):
        code=list("a"*len(text))
        for i in range(len(text)):
            code[i]=self.letter_return(get_letter_position(text[i]))
            self.rotate()
        return code
        
    def rotate(self):
        first_letter=self.order[0]
        remaining=self.order[1:]
        self.order=remaining+first_letter
        text=[]
        for letter in self.order:
            if letter == 'a':
                new_letter ='z'
            else:
                new_letter=chr(ord(letter)-1)
            text.append(new_letter)
        self.order="".join(text)
 
def get_letter_position(letter):
  if not letter.isalpha() or not letter.islower():
    return "Invalid input: not a lowercase letter"
  return ord(letter) - 96
  
switchboard=board()
num=int(input("how many pairs of letters do you want to swap on the switchboard: "))
for i in range(num):
    switchboard.letter_change()

rotor1=rotor()
num=int(input("how many pairs of letters do you want to swap on the rotor: "))
for i in range(num):
    rotor1.letter_change()

text=input("enter text to encode in lower case: ")
code=switchboard.encode(text)
code=rotor1.encode(code)
print("".join([str(x) for x in code]))
