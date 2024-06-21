# התקנת WSL על מחשב מארח מסוג Windows 11 והגדרת האפשרות לשימוש ב GUI  
## שלב ראשון
### התקנה והגדרה של WSL על מערכת Windows 11  
* תחילה יש לוודא שהפיצ'רים ההכרחיים פעילים, נפתח חלון PowerShell חדש בהרשאות אדמין ` Get-WindowsOptionalFeature -Online | Where-Object { $_.FeatureName -like "*linux*" -or $_.FeatureName -like "*hyper* | Enable-WindowsOptionalFeature" }`
* כאשר יופיע הפרומפט נבצע אתחול למחשב
* נפתח חלון PowerShell ונוודא שאנו מריצים את הגרסה העדכנית ביותר `wsl --update`
* לחלופין ניתן להוריד את עדכון הקרנל בצורה ידנית [מכאן](https://learn.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
* מומלץ לבצע אתחול נוסף למחשב
* נגדיר ש WSL ירוץ בגרסה העדכנית ביותר `wsl --set-default-version 2`
* נתקין מכונת WSL מסוג Kali Linux [מכאן](https://apps.microsoft.com/store/detail/kali-linux/9PKR34TNCV07)
* נבחר שם Unix חדש ונגדיר סיסמא
* לאחר שהמכונה תעלה, נבצע עדכון ושדרוג למערכת `sudo apt update -y && sudo apt upgrade -y`
* נבצע ריענון לטרמינל `source .bashrc`
## שלב שני
### התקנה של Win-KeX (כלי המאפשר שימוש ב GUI על גבי WSL 2)
* בתוך WSL הרץ בטרמינל `sudo apt install -y kali-win-kex`
## שלב שלישי
### פתיחת סישן חדש ב kex
* התחברות דרך הטרמינל `kex --win -s`
* התחברות דרך שורת הפקודה במחשב Windows המארח `wsl -d kali-linux kex --win -s`