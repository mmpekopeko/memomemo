README.md



// eslint.config.mjs

```mjs
import js from "@eslint/js";
import nextPlugin from "@next/eslint-plugin-next";
import tseslint from "typescript-eslint";
import importPlugin from "eslint-plugin-import";

export default [

  // 1️⃣ ベース設定
  js.configs.recommended,

  ...tseslint.configs.recommended,

  {
    plugins: {
      "@next/next": nextPlugin,
      import: importPlugin,
    },

    rules: {
      /*
       * 🚫 function宣言を完全禁止（Arrow Functionのみ許可）
       */
      "func-style": ["error", "expression"],

      /*
       * 🚫 通常ファイルでは default export を禁止
       */
      "import/no-default-export": "error",

      /*
       * const を強制
       */
      "prefer-const": "error",

      /*
       * React 17+ JSX Transform対応
       */
      "react/react-in-jsx-scope": "off",
    },
  },

  // 2️⃣ Next.js 規定ファイルのみ default export 許可
  {
    files: [
      "app/**/page.tsx",
      "app/**/layout.tsx",
      "app/**/loading.tsx",
      "app/**/error.tsx",
      "app/**/not-found.tsx",
      "app/**/route.ts",
    ],
    rules: {
      "import/no-default-export": "off",
    },
  },
];
```

============================
```mjs
eslint.config.mjs
gemini

import typescriptParser from "@typescript-eslint/parser";
import typescriptPlugin from "@typescript-eslint/eslint-plugin";
import importPlugin from "eslint-plugin-import";

export default [
  {
    // 全ての TypeScript ファイルに適用
    files: ["**/*.ts", "**/*.tsx"],
    languageOptions: {
      parser: typescriptParser,
      parserOptions: {
        // プロジェクトのルートにある tsconfig.json を参照
        project: "./tsconfig.json",
      },
    },
    plugins: {
      "@typescript-eslint": typescriptPlugin,
      import: importPlugin,
    },
    rules: {
      // 1. 関数宣言を禁止し、アロー関数(const)を強制
      "func-style": ["error", "expression"],
      
      // 2. 原則として Default Export を禁止 (Named Export ③ を推奨)
      "import/no-default-export": "error",
      
      // 3. 'export default function ...' の形式を禁止 (②の分離書きを強制)
      "no-restricted-syntax": [
        "error",
        {
          "selector": "ExportDefaultDeclaration > FunctionDeclaration",
          "message": "Export default function is prohibited. Use 'const Name = () => {}; export default Name;' instead."
        }
      ],
    },
  },
  {
    // Next.js の規定ファイルのみ、default export の禁止を解除 (②を許可)
    // src/app 構成とルート app 構成の両方に対応
    files: [
      "**/app/**/page.tsx",
      "**/app/**/layout.tsx",
      "**/app/**/loading.tsx",
      "**/app/**/error.tsx",
      "**/app/**/not-found.tsx",
      "**/app/**/template.tsx",
      "**/app/**/default.tsx",
      "**/app/**/route.ts"
    ],
    rules: {
      "import/no-default-export": "off",
    },
  },
];

```


