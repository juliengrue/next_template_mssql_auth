## Getting Started

### Installation du projet

1. Créez une clef SSH pour votre compte Azure :

    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

    a. Puis ajoutez la clef SSH à votre compte Azure :

    - [Tutoriel](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-keys-detailed)

    b. Pensez à changer votre config SSH :

    - Ouvrez le fichier `~/.ssh/config`:
    - Ajoutez les lignes :

    ```
    Host ssh.dev.azure.com

    IdentityFile ~/.ssh/<ma-clef-ssh>
    IdentitiesOnly yes
    ```

2. Si ce n'est pas déjà fait, installez PNPM :

-   [PNPM](https://pnpm.io/installation)

```bash
pnpm env use --global 20.18.3
pnpm install
```

### Installation de la BDD

1. Vérifiez que vous avez SQL server installé
    - Si ce n'est pas le cas, installez-le [ici](https://www.microsoft.com/fr-fr/sql-server/sql-server-downloads)
2. Créez une base de données nommée `spareparts` :
   a. Sur Ubuntu :
   i. Connectez-vous à votre SQL server:
   `bash
     sqlcmd -S <server> -U <username> -P <password> -C
     `
   ii. Créez la base de données:
   `sql
     CREATE DATABASE spareparts;
     `
   b. Sur Windows :
   i. Lancer la commande pour créer une instance : `sqllocaldb c spareparts`
   ii. Lancer la commande pour lancer le serveur : `sqllocaldb s spareparts`
   iii. Ouvrez SQL Server Management Studio et connectez-vous à l'instance : `(localdb)\spareparts`
   iv. Créez la base de données spareparts sur SSMS
   v. Activer la BDD autonome :
   `sql
     sp_configure 'contained database authentication', 1;  
     GO  
     RECONFIGURE;  
     GO  
     `
   vi. Kill les processus liés à la BDD :
   `       EXEC sp_who2;
   `
   vii. Kill les processus liés à la BDD :
   `       KILL <SPID>;
   `
   vii. Activer l'option de BDD autonome
   ix. Créer le user avec mot de passe.
3. Copier `env.exemple` par `.env` et remplissez-le avec les informations de votre user.

### BDD de test

1. Créez une base de données nommée `sparepartstest`
    - Connectez-vous à votre SQL server:
    ```bash
    sqlcmd -S <server> -U <username> -P <password> -C
    ```
2. Créez le user sql server de test ainsi que la base de données:

    ```sql
    CREATE DATABASE sparepartstest;
    GO

    CREATE LOGIN testuser WITH PASSWORD = 'Qsdqsdqsd1';
    GO

    USE sparepartstest;
    CREATE USER testuser FOR LOGIN testuser;
    GRANT CONTROL ON DATABASE::sparepartstest TO testuser;
    GO
    ```

3. Copier `env.test.exemple` par `.env.test`
4. Vérifie que les tests arrivent à se connecter à la BDD de test avec `pnpm test`

### Prettier

Sur VSCode, installez l'extension Prettier :

1. Ouvrez le menu des Extensions avec `Ctrl + Shift + X`
2. Recherchez `Prettier` dans la barre de recherche

    ![Prettier plugin example](/docs/prettierpluging.PNG)

3. Installez l'extension
4. Ouvrez les paramètres de l'extension :

    ![Where are prettier settings](/docs/prettiersettings.PNG)

5. Vérifiez les settings suivants :

    ![prettier config path](/docs/prettiersettings1.PNG)

### Commandes

-   `pnpm run dev` : Démarrer le serveur de développement
-   `pnpm run test` : Lancer les tests
-   `pnpm run coverage` : Lance un test coverage avec un rapport html.
-   `pnpm run storybook` : Démarrer le serveur storybook
