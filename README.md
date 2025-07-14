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
    counter=0
    position=0

    def __init__(self,num):
        self.rotor_num=num
        
    def encode(self, text):
        code=list("a"*len(text))
        for i in range(len(text)):
            code[i]=self.letter_return(get_letter_position(text[i]))
            self.counter=self.counter+1
            if self.rotor_num>1:
                if self.counter%(26^(self.rotor_num-1))==0:
                    self.counter=0
                    self.position=self.position+1
                    self.rotate()
            else:
                self.counter=self.counter%26
                self.position=self.position+1
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
num=int(input("How many pairs of letters do you want to swap on the switchboard: "))
for i in range(num):
    switchboard.letter_change()

rotors=int(input("How many rotors do you want?"))
rotor_arr=list("a"*rotors)

for j in range(rotors):
    rotor_arr[j-1]=rotor(j)
    print("How many pairs of letters do you want to swap on rotor number" , j+1 ,": ")
    num=int(input())
    for i in range(num):
        rotor_arr[j-1].letter_change()

d=input("How many pairs of letters do you want a reflector? Y/N: ")
if d=='Y':
    reflector=board()
    num=int(input("How many pairs of letters do you want to swap on the reflector: "))
    for i in range(num):
        reflector.letter_change()
    
code=input("Enter text to encode in lower case: ")
code=switchboard.encode(code)
for j in range(rotors):
    code=rotor_arr[j-1].encode(code)
if d=='Y':
    code=reflector.encode(code)

print("".join([str(x) for x in code]))
