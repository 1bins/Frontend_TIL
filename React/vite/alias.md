# 절대 경로 설정

vite.config.ts
```ts
//...
import path from 'path';

export default defineConfig({
    // ...
    resolve: {
        alias: {
            '@': path.resolve(__dirname, 'src'),
        },
    },
})
```

<br>

tsconfig.json
```json
{
    "compilerOptions": {
        "baseUrl": ".",
            "paths": {
            "@/*": ["src/*"]
        }
    }
}
```
tsconfig.app.json
```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```