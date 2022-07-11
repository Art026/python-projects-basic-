from tkinter import *
from tkinter import messagebox
import primeGenerator


def get_n(p1, q1):
    return p1 * q1


def get_tn(p1, q1):
    return (p1 - 1) * (q1 - 1)


def gcd(x, y):
    if y == 0:
        return x
    else:
        return gcd(y, x % y)


def get_e(tn11):
    e111 = 2
    while gcd(tn11, e111) != 1:
        e111 = e111 + 1
    return e111


def get_d(e11, tn11):
    n11 = 1
    while (tn * n11 + 1) % e11 != 0:
        n11 = n11 + 1
    d11 = (tn11 * n11 + 1) / e11
    return d11


p = primeGenerator.primesInRange(100, 250)
q = primeGenerator.primesInRange(100, 250)
print(p, q)
tn = get_tn(p, q)
n = get_n(p, q)
e = get_e(tn)
d = get_d(e, tn)
print("Public Key:(e,n):", e, n)
print("Private Key:(d,n):", d, n)


def encrypt():
    plaintext = entryvalue.get()
    nv = int(n1.get())
    ev = int(e1.get())
    ctxt = []
    if nv == n and ev == e:
        # plaintext1=converter.PT_CT(plaintext)
        # ciphertext = rsa(plaintext1, e, n)
        for ch in plaintext:
            k = ord(ch)
            ct = (k ** e) % n
            ctxt.append(ct)
        print('Ciphertext:', ctxt)
        # output to the listbox
        listbox.insert(0, ctxt)
    else:
        messagebox.showerror("bye!", "wrong public key!")


def decrypt():
    # get the ciphertext the user input
    ciphertext = entryvalue2.get()
    ciphertext = ciphertext.split(', ')
    print(ciphertext)
    print("d,n:", d, n)
    nv = int(n2.get())
    dv = float(dentist.get())
    print("dv,nv:", dv, nv)
    if nv == n and dv == d:
        # plaintext = rsa(ciphertext, d, n)
        # plaintext1=converter.CT_PT(ciphertext)
        d1=int(d)
        print('d1:',d1)
        print(type(d1))
        ptxt = ""
        for i in ciphertext:
            k=int(i)
            pt = (k ** d1) % n
            ptxt += chr(pt)
        print(ptxt)
        listbox2.insert(0, ptxt)
    else:
        messagebox.showerror("wrong private key!")


root = Tk()
# GUI title                
root.title('RSA')

# first three and encrypt
l = Label(root, text='Input the plaintext')
l.pack()
entryvalue = Entry(root)
entryvalue.pack()

l3 = Label(root, text='n')
l3.pack()
n1 = Entry(root)
n1.pack()

l4 = Label(root, text='e')
l4.pack()
e1 = Entry(root)
e1.pack()

button = Button(root, text="Encrypt", command=encrypt)
button.pack()

show = Label(root, text='Show Ciphertext:')
show.pack()
listbox = Listbox(root, height=2, width=60)
listbox.pack()

# next three with decypt
label = Label(root, text='Input the ciphertext')
label.pack()
entryvalue2 = Entry(root)
entryvalue2.pack()

l5 = Label(root, text='n')
l5.pack()
n2 = Entry(root)
n2.pack()

l6 = Label(root, text='d')
l6.pack()
dentist = Entry(root)
dentist.pack()

button2 = Button(root, text="Decrypt", command=decrypt)
button2.pack()

show2 = Label(root, text='Show Plaintext:')
show2.pack()
listbox2 = Listbox(root, height=3, width=40)
listbox2.pack()

root.mainloop()
