from tkinter import *
import random

class Connect4:
    
    def __init__(self, width, height, window=None):
        self.width = width
        #width of the board
        self.height = height
        #height of the board
        self.Bwidth = width*100
        #width of the GUI board
        self.Bheight = height*100
        #height of the GUI board
        self.data = [] #keeps track data of the board
        
        for row in range(self.height):
            boardRow = []
            for col in range(self.width):
                boardRow += [' ']
                #add a space to each row
            self.data += [boardRow]
        self.rowSize = 6
        #number of rows
        self.columnSize = 7
        #number of columns
        self.padding = 10
        self.footer = 50
        self.diamX = (self.Bwidth/self.columnSize)-self.padding
        self.diamY = (self.Bheight/self.rowSize)-self.padding
        #calculate the diameter of the circles
        self.window = window
        #create the window of GUI
        self.frame = Frame(self.window)
        #create a frame on top of the window
        self.frame.pack(expand = True,fill= X)
        self.scale = Scale(self.frame,orient=HORIZONTAL,\
            showvalue=1,to=7,length=50, \
            command= self.changePly)
        self.scale.pack(expand = True,fill= X,side = LEFT)
        #create a slider for changing plies
        self.qButton = Button(self.frame, text="Quit Game",
        command=self.qButtonAction,font=('Courier', 14, 'bold italic'))
        self.qButton.pack(expand = True,side=RIGHT)

        self.newGameButton = Button(self.frame, text = "New Game",
            command = self.newGameButton,font = ('Courier',14))
        self.newGameButton.pack(expand = True,side=RIGHT)

        self.draw = Canvas(self.window, width=self.Bwidth+self.padding, 
            height=self.Bheight+self.padding+50)
        self.draw.pack()

        self.draw.bind('<Button-1>', self.mouse)
        #get the mouse bind
        self.draw.pack()

        self.circles = []
        self.colors = []

        self.textVar = Text(self.window, height = 2, width = 30,bd=10)
        self.textVar.insert(INSERT, " ")
        self.textVar.pack()

        y = self.padding
        for row in range(self.rowSize):
            circleRow = []
            colorRow = []
            x = self.padding
            for col in range(self.columnSize):
                circleRow += [self.draw.create_oval(x, y,
                    x+self.diamX, y+self.diamY, fill='snow')]
                #Creates a circle at the given coordinates. 
                #It takes two pairs of coordinates; the top left and bottom right 
                #corners of the bounding rectangle for the oval.
                colorRow += ['snow']
                x += self.diamY+self.padding
            self.circles += [circleRow]
            self.colors += [colorRow] 
            y += self.diamX+self.padding

        self.line = 0

    def qButtonAction(self):
        self.window.destroy()

    def newGameButton(self):
        self.clear() 
        self.clearGUI()
        self.draw.delete(self.line)
        self.textVar.delete('1.0',END)

    def playwithGUI(self,x):
        self.compperson = x
        #pass x as AI player in main, assign AI player as self.compperson

    def changePly(self,num):
        self.compperson.ply = int(num)

    def mouse(self,event):
        col = int(event.x/(self.diamX+self.padding))
        row = int(event.y/(self.diamY+self.padding))
        self.addMove(col,'x')
        self.colorMove()
        if self.winsFor('x',True) and not self.isFull():
            self.textVar.insert(END,'You have won \n Congrats!!!')
            self.textVar.tag_configure("center", justify='center')
            self.textVar.tag_add("center", "1.0", "end")
            self.draw.unbind("<Button-1>")
            return 
        self.addMove(self.compperson.nextMove(self),'o')
        self.colorMove()
        if self.winsFor('o',True) and not self.isFull():
            self.textVar.insert(END,'AI Player has won \n Good luck next time!')
            self.textVar.tag_configure("center", justify='center')
            self.textVar.tag_add("center", "1.0", "end")
            self.draw.unbind("<Button-1>")
            return 
        if not self.winsFor('x',True) and not self.winsFor('o',True) and self.isFull():
            self.textVar.insert(END,'This game is a tie \n No one has won') 
            self.textVar.tag_configure("center", justify='center')
            self.textVar.tag_add("center", "1.0", "end")
            self.draw.unbind("<Button-1>")
            return
        print(self)

    def colorMove(self):
        for row in range(self.height):
            for col in range(self.width):
                if self.data[row][col] == 'x':
                    self.draw.itemconfig(self.circles[row][col], fill='black')
                if self.data[row][col] == 'o':
                    self.draw.itemconfig(self.circles[row][col], fill='red')
        self.window.update()

    def vicLine(self,row,col,direction):       
        if direction == 'horizontal':
            #rows stay the same
            #columns go from  left to right
            self.line = self.draw.create_line(2*self.padding+100*col,100*row+self.footer,
                100*(col+4)-self.padding,100*row+self.footer,
                smooth=TRUE,width=10,fill="blue")
            #x and y coordinates of the start and end points of the line
            return 
        if direction == 'vertical':
            #rows go from top to bottom
            #columns stay the same
            self.line = self.draw.create_line(100*col+5*self.padding,100*row+3*self.padding,
                100*col+5*self.padding,100*(row+4)-self.padding,
                smooth=TRUE,width=10,fill ='blue')            
            return 
        if direction =='NWtoSE':
            #rows go from top to bottom
            #columns go from left to right           
            self.line = self.draw.create_line(100*col+5*self.padding,100*row+5*self.padding,
                100*(col+4)-2*self.padding,100*(row+4)-3*self.padding,width=10,fill='blue')
            return 
        if direction == 'NEtoSW':
            #rows go from top to bottom
            #columns go from right to left
            self.line=self.draw.create_line(100*col+5*self.padding,100*row+5*self.padding,
                100*(col-3)+5*self.padding,100*(row+4)-self.footer,
                smooth = TRUE,width=10,fill='blue')
            return 

    def __repr__(self):
        s = ''
        for row in range(self.height):
            s += '|'
            #add a first '|' for each row
            for col in range(self.width):
                s += self.data[row][col] + '|'
            s+= '\n'
        s+= '--'*self.width +'-\n'
        #add a line for '--' seperator 


        for col in range(self.width):
            s += ' ' +str(col %10)
            #add a line for column index
        s += '\n' 
        return s

    def addMove(self,col,ox):
        if self.allowsMove(col):
            for row in range (self.height):               
                if self.data[row][col]!= ' ':                    
                    self.data[row-1][col] = ox
                    #check if the position has a checker, fill in a row ab
                    return           
            self.data[self.height-1][col] = ox
            return 
        else:
            return 

    def clear(self):
        self.data = []
        for row in range(self.height):
            boardRow = []
            for col in range(self.width):
                boardRow += [' ']

            self.data += [boardRow]
        print(self)

    def clearGUI(self):
        for row in range(self.height):
            for col in range(self.width):
                if self.data[row][col] == ' ':
                    #after clearing the board, change all to snow color
                    self.draw.itemconfig(self.circles[row][col],fill='snow')
        self.window.update()
        print("updating the window")
        #get the mouse bind again
        self.draw.bind('<Button-1>', self.mouse)
        self.draw.pack()

    def delMove(self,col):
        for row in range(self.height):
            if self.data[row][col] == 'x' or self.data[row][col] == 'o':              
                self.data[row][col] = ' '
                return
                #exit immediately after one checker
   
    def allowsMove(self,col):
        if 0 <= col < self.width:
            return self.data[0][col] == ' '
            #check if the top row is empty

        return False

    def isFull(self):
        for col in range(self.width):
        #check if the top row of every column is empty
            if self.data[0][col] == ' ':
                    return False
        return True

    def winsFor(self,ox,line=False):
        #horizontal
        for row in range(self.height):
            for col in range(self.width- 3):
                if self.data[row][col] == ox and \
                self.data[row][col+1] == ox and \
                self.data[row][col+2] == ox and \
                self.data[row][col+3] == ox:
                    if line:
                        self.vicLine(row,col,'horizontal')
                    return True
        #vertical
        for row in range(self.height - 3):       
            for col in range(self.width):
                if self.data[row][col] == ox and \
                self.data[row+1][col] == ox and \
                self.data[row+2][col] == ox and \
                self.data[row+3][col] == ox:
                    if line:
                        self.vicLine(row,col,'vertical')
                    return True
        # #NW --> SE
        #top left to bottom right 
        for row in range(self.height -3):
            for col in range(self.width-3):
                if self.data[row][col] == ox and \
                self.data[row+1][col+1] == ox and \
                self.data[row+2][col+2] == ox and \
                self.data[row+3][col+3] == ox:
                    if line:
                        self.vicLine(row,col,'NWtoSE')
                    return True
        #NE --> SW
        for row in range(self.height -3):
            for col in range(3,self.width,1): 
                if self.data[row][col] == ox and \
                self.data[row+1][col-1] == ox and \
                self.data[row+2][col-2] == ox and \
                self.data[row+3][col-3] == ox:
                    if line:
                        self.vicLine(row,col,'NEtoSW') 
                    return True
        return False  

class Player:

    def __init__(self,ox,tbt,ply):   
        self.ox = ox
        self.tbt = tbt
        self.ply = ply

    def nextMove(self,b):
        print('Ply is: ' + str(self.ply)) 
        return self.tiebreakMove(self.scoresFor(b,self.ox,self.ply))

    def scoreBoard(self,b):
        if b.winsFor('x') and b.winsFor('o'):
            return 100
        if not b.winsFor('x') and not b.winsFor('o'):
            return 0
        else:
            return 50

    def scoresFor(self,b,ox,ply):
        scores = []
        for col in range(b.width):           
            if not b.allowsMove(col):
                scores +=[-1]
            else:
                b.addMove(col,ox)                          
                if b.winsFor(ox):
                    scores +=[100]
                elif ply > 1:
                    checker = ' '
                    if ox == 'x':
                        checker = 'o'
                    else:
                        checker = 'x'
                    scores += [100-max(self.scoresFor(b,checker,ply-1))]
                    #scores of one player is 100 subtract max of opponent'score
                else:
                    scores +=[50]
                b.delMove(col)
        return scores   
    
    def tiebreakMove(self,scores):
        x = max(scores)
        L =[]
        for i in range(len(scores)):
            if x == scores[i]:
                L.append(i)
        if self.tbt == 'Left':
            return L[0]
        if self.tbt == 'Right':
            return L[-1]
        if self.tbt == 'Random':
            return random.choice(L)

def main():
    root = Tk()
    root.title("Let's play Connect4")
    b = Connect4(7,6,root)
    aiPlayer = Player('o','Left',0)
    b.playwithGUI(aiPlayer)
    print(b)
    root.mainloop()
if __name__ == '__main__':
    main()
