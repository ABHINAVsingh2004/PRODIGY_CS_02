# PRODIGY_CS_02
PIXEL MANIPULATION TOOL
import cv2
import numpy as np
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk

# Encryption Key (Change for different results)
ENCRYPTION_KEY = 50  

class ImageEncryptorApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Image Encryption Tool")
        self.root.geometry("400x400")
        
        # Buttons
        self.btn_select = tk.Button(root, text="Select Image", command=self.load_image)
        self.btn_select.pack(pady=10)

        self.btn_encrypt = tk.Button(root, text="Encrypt", command=self.encrypt_image, state=tk.DISABLED)
        self.btn_encrypt.pack(pady=10)

        self.btn_decrypt = tk.Button(root, text="Decrypt", command=self.decrypt_image, state=tk.DISABLED)
        self.btn_decrypt.pack(pady=10)

        self.image_label = tk.Label(root, text="No Image Loaded")
        self.image_label.pack(pady=10)

        self.image_path = None  # Stores selected image path

    def load_image(self):
        """Select an image from the computer."""
        file_path = filedialog.askopenfilename(filetypes=[("Image Files", "*.png;*.jpg;*.jpeg")])
        if file_path:
            self.image_path = file_path
            self.display_image(file_path)
            self.btn_encrypt.config(state=tk.NORMAL)
            self.btn_decrypt.config(state=tk.NORMAL)

    def display_image(self, path):
        """Show the selected image in the GUI."""
        image = Image.open(path)
        image = image.resize((200, 200))
        img_tk = ImageTk.PhotoImage(image)

        self.image_label.config(image=img_tk, text="")
        self.image_label.image = img_tk

    def encrypt_image(self):
        """Encrypt the image by modifying pixel values."""
        if self.image_path:
            img = cv2.imread(self.image_path)
            encrypted_img = (img + ENCRYPTION_KEY) % 256  # Simple pixel manipulation
            cv2.imwrite("encrypted_image.png", encrypted_img)
            messagebox.showinfo("Success", "Image Encrypted and saved as encrypted_image.png!")

    def decrypt_image(self):
        """Decrypt the image back to its original form."""
        try:
            img = cv2.imread("encrypted_image.png")
            decrypted_img = (img - ENCRYPTION_KEY) % 256  # Reverse pixel manipulation
            cv2.imwrite("decrypted_image.png", decrypted_img)
            messagebox.showinfo("Success", "Image Decrypted and saved as decrypted_image.png!")
        except Exception as e:
            messagebox.showerror("Error", "No encrypted image found!")

# Run the App
if __name__ == "__main__":
    root = tk.Tk()
    app = ImageEncryptorApp(root)
    root.mainloop()
