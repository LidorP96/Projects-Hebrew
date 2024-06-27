# הרצת מכונה וירטואלית ב Azure עם שולחן עבודה בעזרת xrdp  
## שלב אחד - הקמת סביבה ווירטואלית ב Azure  
* נפתח חלון CloudShell חדש עם סביבת PowerShell
* יצירת Resource Group חדש בשם lab-rg01 באזור ישראל `` New-AzResourceGroup -Name lab-rg01 -Location israelcentral -Force ``
* יצירת רשת וירטואלית\VNET חדשה עם Subnet בודד שיכיל את המכונה הוירטואלית
```
$firstSubnet = New-AzVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix "10.0.0.0/29"
New-AzVirtualNetwork -Name lab-vnet01 -ResourceGroupName lab-rg01 -Location israelcentral -AddressPrefix "10.0.0.0/28" -Subnet $firstSubnet
```
* יצירת כרטיס רשת\NIC חדש עם תעבורה נכנסת בפורט 3389\RDP, ופורט 22\SSH
```
$rule1 = New-AzNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix `
Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    
$rule2 = New-AzNetworkSecurityRuleConfig -Name ssh-rule -Description "Allow SSH" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 -SourceAddressPrefix `
Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22
    
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName lab-rg01 -Location israelcentral -Name `
"kali-nsg01" -SecurityRules $rule1,$rule2
```
* פתח את חלון יצירת מכונה ויראוטלית בפורטל, ושנה את הערכים בלשוניות הבאות:
```
Basics tab:

Subscription name 👉 <sub_name>
Resource group 👉 lab-rg01
Virtual machine name 👉 kali
Region 👉 israelcentral
Availability options 👉 No infrastructure redundancy required
Security type 👉 Standard
Image 👉 Kali 2024.2 - x64 Gen2
Size 👉 Standard_D8s_v3 - 8 vcpus, 32 GiB memory
Authentication type 👉 Password
Username 👉 kali
Password 👉 <choose_yours>
Public inbound ports 👉 SSH
```
```
Disks tab:

OS disk size 👉 128 GiB (P10)
```
```
Netoworking tab:

Virtual netowrk 👉 lab-vnet01
Subnet 👉 subnet01
NIC network security group 👉 kali-nsg01
Delete public IP and NIC when VM is deleted 👉 True
```
## שלב שני - התחברות ב SSH ועדכון המערכת
* התחבר למכונה `ssh vm_name@ip_address`
* לאחר שיופיע הפרומפט לאישור בחר ב `y`
* עדכן את מתקין החבילות `sudo apt update -y`
* עדכן את החבילות במערכת לגרסא העדכנית ביותר `sudo apt full-upgrade -y`
## שלב שלישי - התקנת חבילות והתחברות למכונה בעזרת xrdp
* התקן את שולחן העבודה `apt install kali-desktop-xfce -y`
* בצע אתחול `sudo reboot`
* התקן את תוכנת ההשתלטות `sudo apt install xrdp -y`
* וודא ש xrdp רץ עם עליית המערכת `sudo systemctl enable xrdp`
* הרץ את השירות `sudo systemctl start xrdp`
* וודא ש xrdp משתמש בשולחן העבודה הרצוי `echo "xfce4-session" > ~/.xsession`
* פתח את תוכנת RDP במחשב המקומי, ובצע התחברות בעזרת שם המשתמש וכתובת IP הציבורית של המכונה