#! /usr/bin/env python3                                                                                                                                                                                                                       

import sqlite3

from Crypto.Cipher import AES
from Crypto.Protocol.KDF import PBKDF2


def get_encrypted_data(db_path):

    # choose a database
    conn = sqlite3.connect(db_path)
    cursor = conn.cursor()
    # connect and egt exncypted data
    data = cursor.execute('SELECT action_url, username_value, password_value FROM logins')
    
    return data


# to get rid of padding
def clean(x): 
    return x[:-x[-1]].decode('utf8')


def get_decrypted_data(encrypted_password):

    print("Decrypting the string: {}".format(encrypted_password))

    # trim off the 'v10' that Chrome/ium prepends
    encrypted_password = encrypted_password[3:]

    # making the key
    salt = b'saltysalt'
    iv = b' ' * 16
    length = 16
    iterations = 1
    pb_pass = "peanuts".encode('utf8')

    key = PBKDF2(pb_pass, salt, length, iterations)
    cipher = AES.new(key, AES.MODE_CBC, IV=iv)
    
    decrypted = cipher.decrypt(encrypted_password)
    print(clean(decrypted))


if __name__ == "__main__":
    
    db_path = '/home/garry/.config/chromium/Default/Login Data'
    for url, user, encrypted_password in get_encrypted_data(db_path):
        get_decrypted_data(encrypted_password)
