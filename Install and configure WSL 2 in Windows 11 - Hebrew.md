# התקנת WSL על מחשב מארח מסוג Windows 11 והגדרת האפשרות לשימוש ב GUI  
## שלב ראשון
### התקנה והגדרה של WSL על מערכת Windows 11  
* תחילה נפתח חלון CMD בהרשאות אדמין ונריץ את ההפקודה הבאה בכדי לוודא שהפיצ'רים הנחוצים רצים:
```
dism /online /get-features /format:table | findstr /I "sub virtual"
```
* במידה והפיצ'רים לא רצים, פתח את שורת ה RUN והרץ ``optionalfeatures`` ולאחר מכן בחר בהם והפעל אותם
* נפתח חלון PowerShell ונוודא שאנו מריצים את הגרסה העדכנית ביותר `wsl --update`
* לחלופין ניתן להוריד את עדכון הקרנל בצורה ידנית [מכאן](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
* מומלץ לבצע אתחול נוסף למחשב
* נגדיר ש WSL ירוץ בגרסה העדכנית ביותר `wsl --set-default-version 2`
* נתקין מכונת WSL מסוג Kali Linux בעזרת הפקודה הבאה:
```
wsl --install -d kali-linux
```
* לאחר מכן בחר את שם המשתמש והסיסמא
## שלב שני
### התקנה של Win-KeX (כלי המאפשר שימוש ב GUI על גבי WSL 2)
* בתוך WSL הרץ בטרמינל `sudo apt install -y kali-win-kex`
## שלב שלישי
### פתיחת סישן חדש ב kex
* התחברות דרך הטרמינל `kex --win -s`
* התחברות דרך שורת הפקודה במחשב Windows המארח `wsl -d kali-linux kex --win -s`
## אוטומציה
- לאחר הקמה והתחברות למכונה, פתח חלון טרמינל חדש
- צור קובץ חדש בשם setup.sh בעורך הטקסט נאנו `nano setup.sh`
- העתק את התוכן הבא
```
#!/bin/bash

# Exit if any command fails
set -e

# Update system
echo "Updating System..."
apt update -y && apt upgrade -y
clear

# Installing kex
echo -e "Installing kex...\nIt will take a while..."
sleep 2
apt install -y kali-win-kex
clear

# Strting kex
clear
echo "Kex has been installed !\nNow starting kex..."
echo "You will be prompted to choose a password !"
sleep 5
sudo -u $SUDO_USER kex --win -s
```
- שמור את הקובץ `CTRL+X`
- הוסף הרשאת הרצה לקובץ `chmod +x setup.sh`
- הרץ את הסקריפט `sudo ./setup.sh`
