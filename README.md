# projekt_azure
Folder projekt-azure.zip zawiera aplikację app.py we Flasku.
Plik komendy.txt zawiera listę komend az cli wykorzystanych przy tworzeniu aplikacji (zawarte również poniżej)

W aplikacji wykorzystano zasoby chmurowe: Virtual Machines i App Service

Komendy AZ CLI:

1 - utworzenie VM:
(korzystanie z resource-group: projekt_azure na eastus)

logowanie do azure
C:\Users\astgh>az login

C:\Users\astgh>az account list
[
  {
    "cloudName": "AzureCloud",
    "homeTenantId": "53bc24c5-73ad-421a-9408-3ea555be4a07",
    "id": "c39983dd-2ef5-48f6-a1d7-b4c0f0992f28",
    "isDefault": true,
    "managedByTenants": [],
    "name": "Azure subscription 1",
    "state": "Enabled",
    "tenantId": "53bc24c5-73ad-421a-9408-3ea555be4a07",
    "user": {
      "name": "wrx63909@student.wsb.wroclaw.pl",
      "type": "user"
    }
  }
]

sprawdzenie grupy zasobów
C:\Users\astgh>az group list --query '[].[projekt_azure,eastus]' -o tsv
[].[projekt_azure,eastus]

utworzenie vm
C:\Users\astgh>az vm create --location eastus \
--resource-group projekt_azure --name projekt --size Standard_B1ls \
--image Win2019Datacenter --public-ip-sku Standard \
--admin-username anna --admin-password ************** 

weryfikacja
C:\Users\astgh>az vm list --output table
Name     ResourceGroup    Location    Zones
-------  ---------------  ----------  -------
projekt  PROJEKT_AZURE    eastus


logowanie: ssh anna@23.100.59.3
anna@azure:~$ ls
anna@azure:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.3 LTS
Release:        20.04
Codename:       focal

instalacja nginx
anna@azure:~$ sudo apt-get update
anna@azure:~$ sudo apt-get install -y nginx

 
2 - pobranie aplikacji https://github.com/Astghik1319/projekt_azure na vm do .zip

anna@anna:~$ git clone https://github.com/Astghik1319/projekt_azure/
Cloning into 'projekt_azure'...
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 11 (delta 3), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (11/11), 1.98 KiB | 225.00 KiB/s, done.

anna@anna:~$ realpath projekt_azure.zip
/home/anna/projekt_azure.zip

3 - instalacja az cli linux
anna@anna:~$ curl -sL https://aka.ms/InstallAzureCLIDEB | sudo bash

4 - Tworzenie app service

anna@anna:~$ az appservice plan create --name projektazure --resource-group projekt_azure --sku B1

5 - deploy na app service z zapisanego wcześniej pliku zip
anna@anna:~$ az webapp deploy --resource-group projekt_azure --name projektazure --src-path /home/anna/projekt_azure.zip 
