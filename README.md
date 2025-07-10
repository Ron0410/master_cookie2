
# master_cookie2
# יצירת קובץ פייתון עם שלד ראשוני למשחק Cookie Clicker
# הקובץ הזה יהיה בסיס, ואחר כך נמשיך להרחיב אותו עד לפחות 1000 שורות

file_path = "/mnt/data/cookie_clicker_ultimate.py"

initial_code = '''
"""
🍪 Cookie Clicker Ultimate 🍪
מאת ChatGPT לפי בקשת משתמש
פייתון עם tkinter | קובץ אחד | צבעוני ומגניב | מעל 1000 שורות
"""

import tkinter as tk
from tkinter import messagebox, simpledialog
import json
import os
import random
import time
import threading

# --- קבועים ועיצוב ---
WINDOW_WIDTH = 800
WINDOW_HEIGHT = 600
FONT_LARGE = ("Comic Sans MS", 20)
FONT_MEDIUM = ("Comic Sans MS", 14)
FONT_SMALL = ("Comic Sans MS", 10)
BG_COLOR = "#FFF8DC"
FG_COLOR = "#8B4513"
BTN_COLOR = "#FFDEAD"
DATA_FILE = "cookie_users.json"

# --- פונקציות נתונים ---
def load_data():
    if not os.path.exists(DATA_FILE):
        return {}
    with open(DATA_FILE, "r", encoding="utf-8") as f:
        return json.load(f)

def save_data(data):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=4)

# --- מחלקת משתמש ---
class User:
    def __init__(self, username, password, info=None):
        self.username = username
        self.password = password
        self.cookies = info.get("cookies", 0) if info else 0
        self.booster = info.get("booster", 1) if info else 1
        self.auto_click = info.get("auto_click", 0) if info else 0
        self.club = info.get("club", None) if info else None
        self.achievements = info.get("achievements", []) if info else []
        self.friends = info.get("friends", []) if info else []

    def to_dict(self):
        return {
            "password": self.password,
            "cookies": self.cookies,
            "booster": self.booster,
            "auto_click": self.auto_click,
            "club": self.club,
            "achievements": self.achievements,
            "friends": self.friends,
        }

# --- המשך יבוא... (נכניס עוד מאות שורות בהמשך) ---

if __name__ == "__main__":
    print("🍪 Cookie Clicker Ultimate טוען...")
    # בקרוב תיפתח החלונית הגרפית
'''

# שמירה ראשונית
with open(file_path, "w", encoding="utf-8") as f:
    f.write(initial_code)

file_path
