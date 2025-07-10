# ממשיכים להרחיב את קובץ Cookie Clicker Ultimate עם חלק משמעותי מהלוגיקה של המשחק
# נוסיף ממשק גרפי בסיסי, קליקים, שדרוגים ואלמנטים צבעוניים

more_code = '''
# --- מחלקת המשחק הראשי ---
class CookieClickerApp:
    def __init__(self, master):
        self.master = master
        self.master.title("🍪 Cookie Clicker Ultimate")
        self.master.geometry(f"{WINDOW_WIDTH}x{WINDOW_HEIGHT}")
        self.master.configure(bg=BG_COLOR)
        self.data = load_data()
        self.user = self.login()
        if not self.user:
            self.master.destroy()
            return

        self.auto_click_running = False
        self.create_ui()
        self.update_ui()
        self.start_auto_clicker()

    def login(self):
        username = simpledialog.askstring("התחברות", "שם משתמש:")
        if not username:
            return None
        password = simpledialog.askstring("התחברות", "סיסמה:", show="*")
        if not password:
            return None

        if username in self.data:
            if self.data[username]["password"] != password:
                messagebox.showerror("שגיאה", "סיסמה שגויה")
                return None
            return User(username, password, self.data[username])
        else:
            self.data[username] = {
                "password": password
            }
            return User(username, password)

    def create_ui(self):
        self.header = tk.Label(self.master, text="🍪 Cookie Clicker Ultimate 🍪", font=FONT_LARGE, bg=BG_COLOR, fg=FG_COLOR)
        self.header.pack(pady=10)

        self.status_label = tk.Label(self.master, text="", font=FONT_MEDIUM, bg=BG_COLOR, fg=FG_COLOR)
        self.status_label.pack(pady=5)

        self.cookie_btn = tk.Button(self.master, text="🍪 לחץ על העוגייה!", command=self.click_cookie, font=FONT_LARGE, bg=BTN_COLOR)
        self.cookie_btn.pack(pady=15)

        self.shop_btn = tk.Button(self.master, text="🛍️ חנות", command=self.open_shop, font=FONT_MEDIUM, bg=BTN_COLOR)
        self.shop_btn.pack()

        self.club_btn = tk.Button(self.master, text="🏆 מועדון", command=self.join_club, font=FONT_MEDIUM, bg=BTN_COLOR)
        self.club_btn.pack(pady=5)

        self.save_btn = tk.Button(self.master, text="💾 שמור", command=self.save_user, font=FONT_MEDIUM, bg=BTN_COLOR)
        self.save_btn.pack(pady=10)

    def update_ui(self):
        self.status_label.config(
            text=f"{self.user.username} | 🍪 עוגיות: {self.user.cookies} | 🔁 בוסטר: x{self.user.booster} | ⚙️ אוטומטי: {self.user.auto_click}/שנ'"
        )

    def click_cookie(self):
        gain = self.user.booster
        self.user.cookies += gain
        self.check_achievements()
        self.update_ui()

    def open_shop(self):
        shop_win = tk.Toplevel(self.master)
        shop_win.title("🛍️ חנות השדרוגים")
        shop_win.configure(bg=BG_COLOR)
        shop_win.geometry("400x300")

        def buy_booster():
            cost = 50 * self.user.booster
            if self.user.cookies >= cost:
                self.user.cookies -= cost
                self.user.booster += 1
                messagebox.showinfo("שדרוג!", f"קנית בוסטר חדש! x{self.user.booster}")
                self.update_ui()
                shop_win.destroy()
            else:
                messagebox.showwarning("אין מספיק עוגיות", f"צריך {cost} עוגיות")

        def buy_auto_click():
            cost = 100 + self.user.auto_click * 150
            if self.user.cookies >= cost:
                self.user.cookies -= cost
                self.user.auto_click += 1
                messagebox.showinfo("אוטוקליק!", f"קנית אוטוקליקר חדש! {self.user.auto_click}/שנ'")
                self.update_ui()
                shop_win.destroy()
            else:
                messagebox.showwarning("אין מספיק עוגיות", f"צריך {cost} עוגיות")

        tk.Button(shop_win, text="שדרוג בוסטר 🍪", font=FONT_MEDIUM, bg=BTN_COLOR, command=buy_booster).pack(pady=10)
        tk.Button(shop_win, text="קנה אוטוקליק 🤖", font=FONT_MEDIUM, bg=BTN_COLOR, command=buy_auto_click).pack(pady=10)

    def join_club(self):
        club = simpledialog.askstring("מועדון", "שם המועדון שתרצה להצטרף אליו:")
        if club:
            self.user.club = club
            messagebox.showinfo("הצטרפות", f"הצטרפת למועדון: {club}")
            self.update_ui()

    def check_achievements(self):
        milestones = [10, 100, 1000, 5000, 10000]
        for m in milestones:
            if self.user.cookies >= m and f"{m}_cookies" not in self.user.achievements:
                self.user.achievements.append(f"{m}_cookies")
                messagebox.showinfo("🎉 הישג!", f"הרווחת {m} עוגיות!")

    def start_auto_clicker(self):
        def run_auto():
            self.auto_click_running = True
            while self.auto_click_running:
                time.sleep(1)
                if self.user.auto_click > 0:
                    self.user.cookies += self.user.auto_click
                    self.update_ui()
        thread = threading.Thread(target=run_auto, daemon=True)
        thread.start()

    def save_user(self):
        self.data[self.user.username] = self.user.to_dict()
        save_data(self.data)
        messagebox.showinfo("נשמר", "המשחק נשמר בהצלחה!")

# --- הפעלת המשחק ---
if __name__ == "__main__":
    root = tk.Tk()
    app = CookieClickerApp(root)
    root.mainloop()
'''

# הוספת הקוד לקובץ הקיים
with open("/mnt/data/cookie_clicker_ultimate.py", "a", encoding="utf-8") as f:
    f.write(more_code)
