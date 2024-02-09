from tkinter import *
from tkinter import scrolledtext
from tkinter import simpledialog
from tkinter.messagebox import showinfo
from tkinter.filedialog import askopenfilename,asksaveasfilename
import os
def newfile():
    global file
    root.title("Untitled-notepad")
    file = None
    textarea.delete(1.0,END)
def openfile():
    global file
    file = askopenfilename(defaultextension=".txt",filetypes=[("All Files","*.*"),("Text Documents","*.txt")])
    if file == "":
        file= None
    else:
        root.title(os.path.basename(file) + "- Notepad")    
        textarea.delete(1.0,END)
        f = open(file,"r")
        textarea.insert(1.0,f.read())
        f.close()
def save_as_file():
    global file
    if file == None:
        file = asksaveasfilename(initialfile='Untitled.txt',defaultextension=".txt",filetypes=[("All Files","*.*"),("Text Documents","*.txt")])
        if file == "":
              file= None
        else:
            f = open(file,"w")
            f.write(textarea.get(1.0,END))
            f.close()
            root.title(os.path.basename(file) + "- Notepad")

def closefile():
    current_tab_index = textarea.index('current')
    textarea.forget(current_tab_index)
def close_window():
    root.destroy()

def cut():
    textarea.event_generate('<<Cut>>')
def copy():
    textarea.event_generate("<<Copy>>")
def paste():
    textarea.event_generate("<<Paste>>")
def find():
    search_text = simpledialog.askstring("Find", "Enter text to find:")
    if search_text:
        start = textarea.search(search_text, "1.0", stopindex=END)
        if start:
            end = f"{start}+{len(search_text)}c"
            textarea.tag_remove("sel", "1.0", END)
            textarea.tag_add("sel", start, end)
            textarea.mark_set("insert", end)
            textarea.see("insert")
        else:
            showinfo("Find", f"Cannot find '{search_text}'")

def replace():
    search_text = simpledialog.askstring("Replace", "Enter text to find:")
    if search_text:
        replace_text = simpledialog.askstring("Replace", f"Replace '{search_text}' with:")
        if replace_text:
            start = textarea.search(search_text, "1.0", stopindex=END)
            while start:
                end = f"{start}+{len(search_text)}c"
                textarea.delete(start, end)
                textarea.insert(start, replace_text)
                start = textarea.search(search_text, end, stopindex=END)
def delete():
    textarea.delete(1.0,END)
def selectall():
    textarea.tag_add("sel", "1.0", "end")
    textarea.tag_raise("sel")
    
def about():
    showinfo("Notepad","Notepad by Code with ria")
def zoom():
    zoom_factor = simpledialog.askfloat("Zoom", "Enter zoom factor (e.g., 1.5 for 150%):", minvalue=0.1)
    if zoom_factor:
        textarea.config(font=("TkDefaultFont", int(zoom_factor * 12)))

def update_status_bar(event=None):
    character_count = len(textarea.get(1.0, 'end-1c'))
    status_var.set(f"Character Count: {character_count}")





root = Tk()
root.title("Notepad")
root.geometry("567x456")

textarea = Text(root,font="lucida 12")
file= None
menubar = Menu(root)
filemanu = Menu(menubar,tearoff=0)
filemanu.add_command(label='New',command=newfile)
filemanu.add_command(label='Open',command=openfile)

filemanu.add_command(label='Save',command=save_as_file)
filemanu.add_command(label='Close tab',command=closefile)
filemanu.add_command(label='Close window',command=close_window)
root.config(menu=menubar)
menubar.add_cascade(label='File',menu=filemanu)
editmanu = Menu(menubar,tearoff=0)
editmanu.add_command(label='Cut',command=cut)
editmanu.add_command(label='Copy',command=copy)
editmanu.add_command(label='Paste',command=paste)
editmanu.add_command(label='Delete',command=delete)
editmanu.add_command(label='Find',command=find)
editmanu.add_command(label='Replace',command=replace)
editmanu.add_command(label='Select All',command=selectall)
menubar.add_cascade(label='Edit',menu=editmanu)
Viewmenu = Menu(menubar,tearoff=0)
Viewmenu.add_command(label="Zoom",command=zoom)
menubar.add_cascade(label="View",menu=Viewmenu)

helpmenu = Menu(menubar,tearoff=0)
helpmenu.add_command(label="About",command=about)
menubar.add_cascade(label="Help",menu=helpmenu)

textarea.pack(fill=BOTH,expand=True)
scorl = Scrollbar(textarea)
scorl.pack(side='right',fill=Y)
scorl.config(command=textarea.yview)
textarea.config(yscrollcommand=scorl.set)

status_var = StringVar()
status_bar = Label(root, textvariable=status_var, bd=1, relief=SUNKEN, anchor=W)
status_bar.pack(side=BOTTOM, fill=X)

# Update status bar on content change
textarea.bind('<KeyRelease>', update_status_bar)

# Initial update of status bar
update_status_bar()

root.mainloop()
