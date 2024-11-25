# To Do List: Documentation
Docs:
- [Function expression & function declaration](#function-expression--function-declaration)
- [Middleware](#middleware)
- [JWT](#jwt)
- [HTTP](#http)
- [Chain of responsibility](#chain-of-responsibility)
- [Singleton](#Singleton)
- [Git Commands](#git-commands)
- [TypeScript](#typescript)
- [Пагінація](#пагінація)
- [SOLID](#solid)
- [Кодування](#кодування)
- [Шифрування](#шифрування)
- [Хешування](#хешування)

## Function expression & function declaration
### Function declaration
function created as a separate element of main code:
```bash
function sum(a, b) {
  return a + b;
}
```



### Function expression
function created inside of another expression: 
```bash
let sum = function(a, b) {
  return a + b;
};
```
### Main differences between function expression & function declaration
- A function declaration can be reached at any point of the code flow;
- A function expression can be used only after it is created, which means only after the expression;
- In a strict mode function declaration can be called only inside the block of code where it's declared;
- Function declaration has it's own context of use 'this'. Meaning 'this' will differ from the way function was called; 

## Middleware
Middleware is a software layer which enables communication between components of code. 


## JWT
JSON Web Token is an internet standard for creating encryption token with optional signature.
Token is a base64 string, which consists of 3 parts - 
1. `header` - general information of algorithm;
2. `payload` - information that is signed in the token; 
3. `signature` - A cryptographic part that is generated based on a secret known only to the system that created this signature. Also, this system has some secret that helps generate a more unique signature that is difficult to duplicate;

### Installation of JWT

```bash
$ npm install jsonwebtoken
```
calling JWT in back-end code, where you'll use JWT for creating token
```bash
const jsonwebtoken = require("jsonwebtoken")
```
### The example of creating token:
```bash
loginUser(email, password){
        let hashpassword = createHash('sha256').update(password).digest('hex');
    # (1)
        let user = this.database.users.find(user => user.email === email && user.password === hashpassword)
    # (2)
        if (user){
            return {
            # (3)
                token: jsonwebtoken.sign({ 
                    id: user.id
                }, 'secret')
            };
        }
        else{
            throw new WrongCredentials();
        }
    }
```
In this example we:
1. Find the user in `database` by it's `user.email` and `user.password`;
2. Check if the user exists;
3. If yes, we create the token for `user.id` using signature `"secret"`;


### The example of token decryption:

```bash
const jsonwebtoken = require("jsonwebtoken")

module.exports = (ctx, next) => {
    # (1)
        const token = ctx.headers.authorization;
    # (2)
        const decoded = jsonwebtoken.verify(token, 'secret');
    # (3)
        ctx.userId = decoded.id;
    next();
}
```
In this example we can see:
1. We get our token from headers and put it in `token`;
2.  We decode `token` using `jsonwebtoken.verify` and put the decoded token in `decoded`;
3. We add the decrypted token as a property `userId` to `ctx`.

## HTTP
The Hypertext Transfer Protocol (HTTP) is the foundation of the World Wide Web, and is used to load webpages using hypertext links.

### HTTP Methods:
1. `POST` request indicates that the client is submitting information to the web server;
2. `GET` request expects information back in return.

### HTTP Headers:
HTTP headers contain text information stored in key-value pairs, and they are included in every HTTP request

Some of the most commonly used request headers are:
- *Accept* - defines the media types that the client is able to accept from the server. For instance, Accept: `application/json`, `text/html` indicates that the client prefers JSON or HTML responses.
- *Authorization* - is used to send the client’s credentials to the server when the client is attempting to access a protected resource. For instance, the client might include a `JSON Web Token (JWT)` as the value of the header, which the server will then verify before returning the requested resource.

### HTTP request body:
The body of a request is the part that contains the ‘body’ of information the request is transferring. The body of an HTTP request contains any information being submitted to the web server, such as a username and password, or any other data entered into a form.

### HTTP response:
An HTTP response is what client receive from an Internet server in answer to an HTTP request.

HTTP response contains:

- *an HTTP status code*
- HTTP response headers
- HTTP body

### HTTP status code:
HTTP status code is a 3-digit code used to indicate whether an HTTP request has been successfully completed. Status codes are broken into the following 5 blocks:

- 1xx Informational
- 2xx Success
- 3xx Redirection
- 4xx Client Error
- 5xx Server Error


## Chain of Responsibility 
Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

## Git Commands

### Git Init
Initializes a new Git repository in your current directory. This creates a .git folder that tracks all your project changes.

``` bash
git init
```

### Git Status
Shows the status of your working directory, such as files that have been modified, files staged for the next commit, and untracked files.

```bash
git status
```
### Git Add
Stages changes (new files, modifications, or deletions) that you want to include in the next commit. Files remain in the staging area until they are committed.

```bash
git add file1.txt           # Add specific file
git add .                   # Add all changes in the current directory
git add *.js                # Add all .js files
```

### Git Commit
Records the changes in the repository with a descriptive message about what has been done. Commits create a snapshot of your project.

```bash
git commit -m "Initial commit"  # Add a message describing the commit
git commit -a -m "Update files" # Automatically stage and commit all tracked changes
```
### Git Push
Uploads your local repository changes to a remote repository (such as GitHub or GitLab).

```bash
git push origin main          # Push local changes to the main branch
git push origin feature-branch # Push local changes to a specific branch
```

### Git Pull
Fetches changes from the remote repository and merges them into your current branch. This is useful when you want to update your local repository with changes made by others.

```bash
git pull origin main
```
### Git Fetch
Downloads changes from the remote repository but does not merge them into your current branch. You need to merge manually using `git merge`.

```bash
git fetch origin
```

### Git Merge
Merges changes from one branch into your current branch. Usually used after fetching changes or when completing work on a feature branch.

```bash
git merge feature-branch
```

### Git Branch
Lists all branches in your repository. You can also use this command to create or delete branches.

```bash
git branch              # List all branches
git branch new-feature  # Create a new branch
git branch -d old-branch # Delete a branch
```

### Git Checkout
Switches between branches or restores files to a specific state. It can also be used to create a new branch and switch to it.

```bash
git checkout main                # Switch to the main branch
git checkout -b new-feature       # Create and switch to a new branch
git checkout -- file.txt          # Restore a file to the latest committed state
```

### Git Log
Shows the commit history for your repository. It lists past commits with their commit messages, authors, and timestamps.

```bash
git log
git log --oneline                # View commits in a simplified one-line format
```

### Git Reset
Unstages changes or resets your repository to a previous commit. It can modify both the staging area and the working directory.

```bash
git reset HEAD file.txt   # Unstage a file
git reset --hard          # Reset all changes, including the working directory
git reset --soft HEAD~1   # Reset to the previous commit, but keep changes in the working directory
```

### Git Rebase
Moves or combines a sequence of commits to a new base commit. Useful for integrating changes without creating merge commits.

```bash
git rebase main
```

## TypeScript
1. Download TS library
```bash
npm install typescript --save-dev
```
2. TS транспілюється в JS.
3. `@Types/node` друга бібліотека - типізовані бібліотеки NodeJS (рідні).
4. `Package.json`
додати скрипт
```json
  "scripts": {
    "tsc": "tsc"
  },
  ```
  5. Ініціалізувати `tsconfig.json` запустивши в терміналі
  ```bash
npm run tsc -- --init
```
6. Налаштувати в `tsconfig.json` властивість `rootDir`, це шлях до всіх ts файлів
7. Налаштувати властивість outDir, це шлях куди буде скомпільовано результуючий js код.
8. Написати в скрипти `build` скрипт
```json
"build": "tsc"
```

## Ініціалізація нового проекту 
1. Ініціалізувати проєкт 
``` json
npm init -y
```
 Скачати бібліотеки. 
```json
npm install (i) libName
```
3. Налаштувати скрипти запуску серверу `(package.json scripts розділ)`
3. Створити структуру папок для бекенду
4. Створюємо `index.js`. Імпортуємо потрібні біліотеки і ініціалізуємо сервер koa.
5. Створити нову базу даних.
6. Спроєктувати структуру таблиць БД.
7. Створити міграції
8. Провести міграції
9. Створити конекшн до БД
10. Написання бізнес логіки


## Пагінація
Пагінація - це процес розділення великої кількості даних на окремі сторінки для полегшення навігації та удосконалення користувацького досвіду. 

```sql
`
    SELECT *
    from items i 
    inner join categories c on i.category_id = c.id
    limit  $1
    offset $2;
`

```

## SOLID
### S - single responsibility 
Принцип єдиної відповідальності.
Кожен клас має свою зону відповідальності. Вчитель не може бути учнем.

### O - open closed 
Принцип відкритості закритості. Класи повинні бути відкриті для розширення але закриті для модифікації

### L - Barbara Liskov
 Якщо в тебе є клас Батько і є клас Син, то заміна усіх класів Батька на клас Син не має повпливати на роботу системи

### I - interface segregation
Ти маєш надавати тільки той функціонал, який потребує частина програми для свого існування
``` ts
class Notifications {
  type: 'EMAIL' | 'SMS'
  message: string
  receiver: string
}

class NotificationRepository implements ISMSRepo, IEMAILReport {
  sendEmail(dto) {
    // do smth
  }

  sendSms() {
    // do smth
  }
}

interface ISMSRepo {
  sendSms: () => void
}

interface IEMAILReport {
  sendEmail: (dto) => void
}

class SMSService {
  constructor(private readonly repo: ISMSRepo) {}

  sendSMS() {
    this.repo.sendSms()
  }
}

class MailService {
  constructor(private readonly repo: IEMAILReport) {}

  sendEMAIL() {
    this.repo.sendEmail({})
  }
}
```

## D - Dependency inversion 
- інверсія залежностей

`Принцип формулюється наступним чином:`
Модулі вищого рівня не повинні залежати від модулів нижчого рівня. Обидва типи модулів повинні залежати від абстракцій.
Абстракції не повинні залежати від деталей реалізації. Деталі реалізації повинні залежати від абстракцій.

```ts
import * as crypto from 'node:crypto';

class CryptoService1 implements ICrypto {
  genRandomInt() {
    const arr: number[] = []
    for (let i = 0; i < 6; i++) {
      arr.push(Math.floor(Math.random() * 6))
    }
    return arr.join('')
  }
}

class CryptoService2 implements ICrypto {
  genRandomInt() {
    return String(crypto.randomInt(100000, 900000));
  }
}

interface ICrypto {
  genRandomInt: () => string
}

class Service {
  constructor(private readonly crypto: ICrypto) {}

  sendEmail() {
    const code = this.crypto.genRandomInt()

    // send mail ... using code
  }
}

const crypto1 = new CryptoService1()
const crypto2 = new CryptoService2()
const service = new Service(crypto2)
```

## Кодування
- представлення даних в іншому форматі. Можливі варіанти - `base64`, `base32`, `base16` під різні потреби.
- також використовується для передачі зображень та файлів в текстовому форматі. 

Найчастіше використовується `base64` через те, що на 6 бітів використовується один символ (велика чи маленька буква, цифра) і вага закодованих даних буде оптимальною (не буде занадто великих втрат ваги при кодуванні).

## Шифрування
- основна задача шифрування - заховати дані від зовнішніх користувачів. Код можна розшифрувати лише маючи необхідний ключ.

- шифрування може бути двох видів: симетричне та асиметричне

### Симетричне шифрування 
- використовується один ключ для того, щоб зашифрувати та розшифрувати дані

### Асиметричне шифрування 
- використовується два ключі, один публічний, щоб зашифрувати та другий приватний, щоб розшифрувати дані. Обидва, приватний та публічний ключ можуть як зашифровувати так і розшифровувати дані.

Наприклад: https
Також два ключі шифрування можуть використовуватись для обміну повідомленнями.

## Хешування 
- це переведення інформації в рядок коду, задля зменшення обсягу збереженої інформації та перевірки на достовірність. 

- Основна властивість - код hash не можливо розшифрувати назад. 

- Також на один інпут буде незмінно один і той самий hash. `Дуже рідко` два різні інпути можуть дати однаковий hash .



