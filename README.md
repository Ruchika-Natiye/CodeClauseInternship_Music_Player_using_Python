# Music_Player_Python

Here is the code:

from tkinter import*
from PIL import Image,ImageTk
from pygame import mixer
mixer.init()
import tkinter.messagebox
from tkinter import filedialog
import os
from mutagen.mp3 import MP3
import time                                    
class musicplayer:
    def __init__(self,Tk):
        self.root=Tk
        self.root.title('Music_Player')
        self.root.geometry("700x500")
        #resizable window off
        #self.root.resizable(0,0)
        self.root.configure(background="black")
        #Openfile
        def Openfile():
            global filename
            filename=filedialog.askopenfilename()
        #Menu
        self.menubar=Menu(self.root)
        self.root.configure(menu=self.menubar)
        self.submenu=Menu(self.menubar,tearoff=0)
        self.menubar.add_cascade(label="file",menu=self.submenu)
        self.submenu.add_command(label="open",command=Openfile)
        self.submenu.add_command(label="Exit",command=self.root.destroy)
        def About():
            tkinter.messagebox.showinfo("About us","Music player is created by Ruchika Natiye")
        self.submenu2=Menu(self.menubar,tearoff=0)
        self.menubar.add_cascade(label="Help",menu=self.submenu2)
        self.submenu2.add_command(label="About",command=About)
        #adding Label--
        self.filelabel=Label(text="Select And Play",fg="black",bg="yellow",font=28)
        self.filelabel.place(x=270,y=20)
        
        
        #adding image---
        self.photo=ImageTk.PhotoImage(file="f1.jpg")
        photo=Label(self.root,image=self.photo,bg="white").place(x=40,y=50)
        #label
        self.label1=Label(self.root,text="Lets play music!",bg="blue",fg="white",font=26)
        self.label1.pack(side=BOTTOM,fill=X)
        # play function
        def playmusic():
          try:
              paused
          except NameError:
              try:
                  mixer.music.load(filename)
                  mixer.music.play()
                  self.label1['text']="Music_Playing..."
                  songinf()
                  length_bar()
                  self.im1=ImageTk.PhotoImage(file="Ruchika1.png")
                  self.im2=ImageTk.PhotoImage(file="Ruchika2.png")
                  self.im3=ImageTk.PhotoImage(file="Ruchika3.jpg")
                  self.im4=ImageTk.PhotoImage(file="Ruchika4.jpg")
                  self.imglabel=Label(self.root,text="",bg="black")
                  self.imglabel.place(x=220,y=250)
                  animation()
              except:
                  
                  tkinter.messagebox.showerror("Error!,file could not found,please try again...")
          else:
              mixer.music.unpause()
              self.label1['text']="Music_Unpaused..."
        
        #for song length
        def length_bar():
            current_time=mixer.music.get_pos()/1000
            convert_current_time=time.strftime("%M:%S",time.gmtime(current_time))
            
            
            song_mute=MP3(filename)
            song_mute_length=song_mute.info.length
            convert_song_mute_length=time.strftime("%M:%S",time.gmtime(song_mute_length))
            self.lengthbar.config(text=f"Total_Length:-:{convert_current_time}:{convert_song_mute_length}")
            self.lengthbar.after(1000,length_bar)
        # label for length bar
        self.lengthbar=Label(self.root,text="Total_length:-00:00",bg="yellow",fg="black",font=24)
        self.lengthbar.place(x=50,y=55)
        def songinf():
            self.filelabel['text']="Current Music:-"+ os.path.basename(filename)
        #play button
        self.photo_B1=ImageTk.PhotoImage(file="play21.png")
        photo_B1=Button(self.root,image=self.photo_B1,bd=0,bg="white",command=playmusic).place(x=100,y=100,width=50,height=50)
        #pause function
        def pausemusic():
            global paused
            paused=TRUE
            mixer.music.pause()
            self.label1['text']="Music_Paused..."
        #pause button
        self.photo_B2=ImageTk.PhotoImage(file="pause11.png")
        photo_B2=Button(self.root,image=self.photo_B2,bd=0,bg="white",command=pausemusic).place(x=200,y=100,width=50,height=50)
        #stop function
        def stopmusic():
            mixer.music.stop()
            self.label1['text']="Music_Stopped..."
        #stop button
        self.photo_B3=ImageTk.PhotoImage(file="stop13.png")
        photo_B3=Button(self.root,image=self.photo_B3,bd=0,bg="white",command=stopmusic).place(x=300,y=100,width=50,height=50)
        
        #Animation
        def animation():
            self.im1=self.im2
            self.im2=self.im3
            self.im3=self.im4
            self.im4=self.im1
            self.imglabel.config(image=self.im1)
            self.imglabel.after(1000,animation)
            
        #function for volume bar
        def volume(vol):
            volume=int(vol)/100
            mixer.music.set_volume(volume)
        
        #mute
        def mute():
            self.scale.set(0)
            self.mute=ImageTk.PhotoImage(file="mute.png")
            mute=Button(self.root,image=self.mute,bd=0,bg="white",command=unmute).place(x=480,y=55)
            self.label1['text']="Music_Mute..."
        def unmute():
            self.scale.set(25)
            self.volimg=ImageTk.PhotoImage(file="vol.png")
            volimg=Button(self.root,image=self.volimg,bg="white",bd=0,command=mute).place(x=480,y=55)
            self.label1['text']="Music_Unmute..."
        #Volume Bar...
        self.scale=Scale(self.root,from_=0,to=100,orient=HORIZONTAL,bg="yellow")
        self.scale.set(25)
        self.scale.place(x=550,y=55)
       
        #Volume Image
        self.volimg=ImageTk.PhotoImage(file="vol.png")
        volimg=Button(self.root,image=self.volimg,bg="white",bd=0,command=mute).place(x=480,y=55)
        
root=Tk()
obj=musicplayer(root)
root.mainloop()
  
