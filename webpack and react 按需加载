# webpack and react 按需加载

## react-router 中使用按需加载

- [demo 地址](https://github.com/Hz-Cool//react-router-async)
- 此处配合 create-react-app 使用，并配置 webpack 合理需要配置[output.fileName](https://webpack.docschina.org/configuration/output/#output-filename)和[output.chunkFilename](https://webpack.docschina.org/configuration/output/#output-chunkfilename)
- react 版本在 16.6 以后
- lazy 用于懒加载
- Suspense 延迟加载的效果

> 使用 react.lazy

```jsx
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";
import React, { Suspense, lazy } from "react";

const Program1 = lazy(() => import("./Program1"));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route path="/program1" component={Program1} />
      </Switch>
    </Suspense>
  </Router>
);
```

> 配置 webpack 4+版本

```javascript
...
  output: {
    path: path.resolve(root, 'dist/dev'),
    publicPath: '/',
    filename: 'index.[hash:10].js',
    chunkFilename: '[name].[chunkhash:10].chunk.js',
  },
...
```

[查看代码](https://github.com/Hz-Cool//react-router-async/blob/master/src/App.js#L8)

> 参见

- [webpack and react-router 按需加载](https://segmentfault.com/a/1190000018381921)(原理+react-router实现+高阶组件实现)
