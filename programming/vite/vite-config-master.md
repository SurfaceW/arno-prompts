You are **vite** expert.

Here is my vite configuration:

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react-swc';
import path from 'path';

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
    // dedupe: ['react', 'react-dom', '@ant-design/icons'],
  },
  plugins: [
    react({
      tsDecorators: true,
      // exclude: /App\.(t|j)sx?$/,
    }),
  ],
  build: {
    // target: 'es2020',
    // 1mb
    // assetsInlineLimit: 1024 * 4,
    rollupOptions: {
      // treeshake: 'smallest',
      input: {
        chat: path.resolve(__dirname, './src/pages/chat.page.tsx'),
        search: path.resolve(__dirname, './src/pages/search.page.tsx'),
      },
      output: {
        entryFileNames: `assets/[name].js`,
        chunkFileNames: `assets/[name].js`,
        assetFileNames: `assets/[name].[ext]`,
      },
    },
  },
});
```

My problem is: this build output a AntDesign Icon with more than 700kb with no tree-shake condition.

"""
dist/assets/search.page.css      0.00 kB │ gzip:   0.02 kB
dist/assets/chat.page.css        1.44 kB │ gzip:   0.65 kB
dist/assets/AntdIcon.css         6.68 kB │ gzip:   2.13 kB
dist/assets/search.js           90.45 kB │ gzip:  33.91 kB
dist/assets/AntdIcon.js        701.71 kB │ gzip: 225.97 kB
dist/assets/chat.js          1,108.10 kB │ gzip: 381.52 kB
"""

How to avoid this and enable tree shake in such config?
