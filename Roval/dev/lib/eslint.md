1、默认情况下，ESLint 会在所有父级目录里寻找配置文件，一直到根目录。如果你想要整个项目都遵循统（同）一约定时，这将会很有用，但另一种情况是把 ESLint 限制到一个特定的项目，在你项目根目录下的 `package.json` 文件或者 `.eslintrc.*` 文件里的 `eslintConfig` 字段下设置 `"root": true`。ESLint 一旦发现配置文件中有 `"root": true`，它就会停止在父级目录中寻找。
```js
{
	"root": true,
}
```
