# Installing NodeJS, NPM & PM2

The standard nodejs version that Ubuntu installs by default is usually pretty outdated. As of writing this the LTS release of NodeJS is 16.13.2. You can verify this yourself at [nodejs.org](https://nodejs.org) and change the first number accordingly. Always use LTS when working with fountainhead cash applications to avoid unforeseen issues.

**Install dependencies and set your desired NodeJS version**

```
sudo apt-get install curl software-properties-common
```
```
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

**Install NodeJS and NPM**

```
sudo apt-get install nodejs
```

**Verify installation**

```
node -v
>v16.13.2

npm -v
>8.3.2
```

**Install PM2 globally**

```
npm install pm2 -g
