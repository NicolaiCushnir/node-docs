*  Change HTML Route in all files : `/languages` in `/user_language` :

```js
const fs = require('fs');
const path = require('path');

const directoryPath = path.join(__dirname, 'public/html');

function updateFooterInFile(filePath) {
    let content = fs.readFileSync(filePath, 'utf-8');

    // Ищем <footer>…</footer>
    const footerRegex = /<footer>[\s\S]*?<\/footer>/gi;
    content = content.replace(footerRegex, (footer) => {
        // Меняем только href="/languages" на href="/user_language"
        return footer.replace('href="/languages"', 'href="/user_language"');
    });

    fs.writeFileSync(filePath, content, 'utf-8');
    console.log(`Updated: ${filePath}`);
}

function walkDirectory(dir) {
    const files = fs.readdirSync(dir);
    for (const file of files) {
        const fullPath = path.join(dir, file);
        const stat = fs.statSync(fullPath);
        if (stat.isDirectory()) {
            walkDirectory(fullPath); // рекурсия для внутренних папок
        } else if (fullPath.endsWith('.html')) {
            updateFooterInFile(fullPath);
        }
    }
}

walkDirectory(directoryPath);
console.log('All done!');
```js