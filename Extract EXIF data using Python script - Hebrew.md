# חילוץ EXIF מתוך תמונות בעזרת סקריפט פייתון  
### מבוא:  
- נתוני מידע EXIF (Exchangeable Image File Format) הם נתונים שנשמרים בתמונות דיגיטליות וכוללים מידע טכני על הצילום כמו תאריך ושעת הצילום, הגדרות מצלמה (כמו מפתח צמצם, מהירות תריס ו-ISO), סוג המצלמה והעדשה, וכן מיקום גיאוגרפי אם יש GPS.
### שלב ראשון - הקמת סביבה  
-  במדריך הבא אני אשתמש ב Kali-Linux, אך ניתן לעקוב גם עם הפצות Debian אחרות
-  תחילה יצרתי מכונת WSL חדשה במחשב ה WIN שלי. ניתן להשתמש במדריך שלי [כאן](https://github.com/LidorP96/Projects-Hebrew/blob/main/Install%20and%20configure%20WSL%202%20in%20Windows%2011%20-%20Hebrew.md)
-  לאחר עליית המכונה, ניצור קובץ חדש בשם exif-install.sh שיכיל את הסקריפט: ``nano exif-install.sh``
-  נעתיק את התוכן הבא אל תוך הקובץ
```
#!/bin/bash

set -e

# update
echo "Updating system..."
sleep 3
apt-get update -y && apt upgrade -y
clear
echo "System updated !"

# installing dependencies
echo "Installing dependencies in 5 seconds..."
sleep 3
apt install -y gallery-dl git python3-pip
clear

# installing python packages
echo -e "Dependencies has been installed !\nNow installing 'pip' packages..."
sleep 3
sudo -u $SUDO_USER python3 -m pip install --upgrade pip
sudo -u $SUDO_USER python3 -m pip install --upgrade Pillow
clear

# creating directories under home folder
echo -e "Done !\nCreating directories to host script and images"
sleep 5
clear
sudo -u $SUDO_USER mkdir -p /home/$SUDO_USER/exif/images
if [[ -d /home/$SUDO_USER/exif && -d /home/$SUDO_USER/exif/images ]]; then
    echo -e "Successfully created this directories under user home folder:\n'exif'\n'images'"
else
    echo "Failed :( "
fi
sleep 3
clear

# cloning git repo with python script
echo "Cloning git repo..."
sleep 3
git clone https://github.com/davidbombal/red-python-scripts.git /home/$SUDO_USER/exif/repo-dir
if [[ $? -eq 0 ]]; then
    echo "Successfully cloned git repo !"
else
    echo "Failed to clone git repo :( "
fi
sleep 3
clear

# moving py script to  path
echo "Moving "exif.py" to 'exif' folder..."
sleep 3
mv /home/$SUDO_USER/exif/repo-dir/exif.py /home/$SUDO_USER/exif
clear

# making exif.py executable
echo "Making 'exif.py' executable..."
sleep 3
clear
chmod +x /home/$SUDO_USER/exif/exif.py
echo -e "Done! When ready, add pictures to the 'images' folder, and then run the 'exif.py' script.\nHave fun !"
```
- נוסיף הרשאת ריצה: ``chmod +x exif-install.sh``
- נריץ את הסקריפט: ``sudo ./exif-install.sh``
- נבצע הורדה של תמונות לדוגמא: ``gallery-dl -D $HOME/exif/images https://www.flickr.com/photos/194419969@N07``
- לבסוף נריץ את הסקריפט מתוך תקיית exif שנוצרה קודם לכן: python3 exif.py
- כפי שנוכל לראות, ניתן לשמור את המידע שחולץ כקובץ או להציגו ישירות מתוך הטרמינל (:
