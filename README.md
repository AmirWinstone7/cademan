from tkinter import *
from tkinter  import  ttk

import messagebox

win=Tk()
win.geometry("%dx%d+%d+%d"%(750,400,400,200))
win.title("ثبت نام")
win.configure(bg="#D8D9DA")

user=[]

#def
def activebtn(e):
    if txt_name.get()=="":
        btn_register.configure(state=DISABLED)
    else:
        btn_register.configure(state=NORMAL)

def onclickregister(e):
    if btn_register.cget("state")==NORMAL:
        dic={"name":txt_name.get(),"family":txt_family.get(),"age":int(txt_age.get())}
        if not exist(dic):
            register(dic)
            insert(dic)
            cleartxt()


def cleartxt():
        txtnamevar.set("")
        txtfamilyvar.set("")
        txtagevar.set("")
        txt_name.focus_set()

def register(value):
    user.append(value)



def insert(value):
        tbl.insert('',"end",value=[value["name"],value["family"],str(value["age"])])


def onclicksearch(e):
    result=search(txt_search.get())
    cleattb()
    for item in result:
        #tbl.insert('',"end",values=[item["name"],item["family"],item["age"]])
        insert(item)
        cleartxt()
        btn_register.configure(state=DISABLED)


def search(value):
    resultlist=[]
    for item in user:
        if item["name"]==value or item["family"] or (item["age"]):
            resultlist.append(item)
    return  resultlist
def cleattb():
    for item in tbl.get_children():
        s=str(item,)
txtnamevar=StringVar()
txtfamilyvar=StringVar()
txtagevar=StringVar()
txtsearchvar=StringVar()
txtTypingvar=StringVar()

def selection(e):
    select1=tbl.selection()
    if select1!=():
        result=tbl.item(select1)["values"]
        txtnamevar.set(result[0])
        txtfamilyvar.set(result[1])
        txtagevar.set(result[2])

def onclickdelet(e):
    select1=tbl.selection()
    if select1!=():
        dict={"name":txt_name.get(),"family":txt_family.get(),"age":(txt_age.get())}
        delete(dict)
        tbl.delete(select1)

def delete(value):
    for item in user:
        if item["name"]==value["name"]and item["family"]==value["family"]and item["age"]==value["age"]:
            user.remove(item)

def clear():
    for item in tbl.get_children():
        sel = str(item,)
        tbl.delete(sel)


def load_and_clear(value):
    for item in tbl.get_children():
        sel = str(item,)
        tbl.delete(sel)
    for item in value:
        tbl.insert('',"end",values=[item["name"],item["family"],str(item["age"])])

def exist(value):
    for item in user:
        if item["name"]==value["name"]and item["family"]==value["family"]and item["age"]==value["age"]:
            return True
        return False

def exist(value):
    for item in user:
        if item["name"]==value["name"]and item["family"]==value["family"]and item["age"]==value["age"]:
            return True
        return False

def onclickupdate(e):
    dialog=messagebox.askyesno("سوال","میخوای اپدیت کنی؟")
    if dialog==True:
        select1=tbl.selection()
        if select1!=():
            result=tbl.item(select1)["values"]
            dic_ghadim={"name":result[0],"family":result[1],"age":result[2]}
            dic_jadid={"name":str(txt_name.get()),"family":str(txt_family.get()),"age":int(txt_age.get())}
            update(dic_ghadim,dic_jadid)
            tbl.item(select1,values=[dic_jadid["name"],dic_jadid["family"],dic_jadid["age"]])
            cleartxt()

def update(value_ghadim,value_jadid):
    i=user.index(value_ghadim)
    user[i]=value_jadid

#txt
txt_name=Entry(win,justify="center",textvariable=txtnamevar,bg="#91C8E4")

txt_name.bind("<KeyRelease>",activebtn)


txt_name.place(x=80,y=50)


txt_family=Entry(win,justify="center",textvariable=txtfamilyvar,bg="#91C8E4")




txt_family.place(x=80,y=90)

txt_age=Entry(win,justify="center",textvariable=txtagevar,bg="#91C8E4")



txt_age.place(x=80,y=130)
txt_search=Entry(win,justify="center",width=30)




txt_search.place(x=350,y=300)
txt_Typing=Entry(win,justify="center",textvariable=txtTypingvar,bg="#91C8E4")




txt_Typing.place(x=80,y=160)


#lbl
lbl_name=Label(win,text="Designed by",bg="#9DB2BF")


lbl_name.place(x=5,y=50)


lbl_family=Label(win,text="Delevoper",bg="#9DB2BF")


lbl_family.place(x=15,y=90)

lbl_age=Label(win,text="First appeared",bg="#9DB2BF")


lbl_age.place(x=0,y=130)
lbl_Typing=Label(win,text="Typing",bg="#9DB2BF")


lbl_Typing.place(x=35,y=160)



#btn
btn_register=Button(win,text="Register",width=16,bg="#00DFA2")
btn_register.configure(state=DISABLED)
btn_register.bind("<Button-1>",onclickregister)
btn_register.place(x=80,y=200)
btn_search=Button(win,text="Search",width=16,bg="#0079FF")

btn_search.bind("<Button-1>",onclicksearch)
btn_search.place(x=210,y=300)
btn_delet=Button(win,text="delet",width=16,bg="#BB2525")

btn_delet.bind("<Button-1>",onclickdelet)
btn_delet.place(x=80,y=230)
btn_update=Button(win,text="update",width=16,bg="orange")

btn_update.bind("<Button-1>",onclickupdate)
btn_update.place(x=80,y=260)

#tbl
tbl=ttk.Treeview(win,columns=("c1","c2","c3","c4"),show="headings",)
tbl
tbl.column("c1",width=70)
tbl.heading("c1",text="Designed by")
tbl.column("c2",width=70)
tbl.heading("c2",text="Developer")
tbl.column("c3",width=70)
tbl.heading("c3",text="First appeared")
tbl.column("c4",width=70)
tbl.heading("c4",text="Typing",)
tbl.bind("<Button-1>",selection)
tbl.column("c1",width=75)


tbl.place(x=250,y=50)
win.mainloop()
