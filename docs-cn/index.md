---
layout: default
title: "Documentation"
---
# react-admin

一个用于搭建管理端应用的前端框架,基于REST/GraphQL的浏览器APIs之上,使用ES6,[React](https://facebook.github.io/react/) 和 [Material Design](https://material.io/). 之前称之为 [admin-on-rest](https://github.com/marmelab/admin-on-rest). 由[marmelab](https://marmelab.com/)开源和维护.


<div style="text-align: center" markdown="1">
<i class="octicon octicon-device-desktop"></i> [示例](https://marmelab.com/react-admin-demo/) -
<i class="octicon octicon-mark-github"></i> [源码](https://github.com/marmelab/react-admin) -
<i class="octicon octicon-megaphone"></i> [新闻](https://marmelab.com/en/blog/#react-admin) -
<i class="octicon octicon-clock"></i> [发布](https://github.com/marmelab/react-admin/releases) -
<i class="octicon octicon-comment-discussion"></i> [支持](http://stackoverflow.com/questions/tagged/react-admin)
</div>

<iframe src="https://player.vimeo.com/video/268958716?byline=0&portrait=0" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="display:block;margin:0 auto"></iframe>

## 特性

* 适用任意后端 (REST, GraphQL, SOAP, 等等.)
* 完备的文档
* 超级快的UI,得益于积极渲染(在服务端返回前渲染)
* 几秒内允许撤销更新和删除的操作
* 支持多种关联关系 (多对一, 一对多)
* 国际化 (i18n)
* 条件格式话
* 可配置主题
* 支持任意验证提供器 (REST API, OAuth, Basic Auth, ...)
* 全特性的数据表格 (排序, 分页, 条件过滤)
* 自定义过滤条件
* 支持任意格式的布局 (简单布局, tab布局, etc.)
* 数据校验
* 自定义actions
* 由很多数据类型 (boolean, number, rich text, 等) 组成的组件库.
* 所见即所得的编辑器
* 自定义数据面板, 菜单, 布局
* 超级容易扩展和重写 (它就是 React 组件)
* 高度定制化的接口
* 能连接到任意后端
* 影响着React生态中最好的库 (Redux, redux-form, redux-saga, material-ui, recompose)
* 能被内嵌到任意的 React 应用
* 灵感来源于这个流行的库 [ng-admin](https://github.com/marmelab/ng-admin) (也是由 marmelab 出品)

## 安装

React-admin 放在 npm 上. 你可以这样安装它 (和它的依赖):

```sh
npm install react-admin
```

## 使用方法

阅读 [指引](./Tutorial.html) 30分钟的介绍. 之后, 前往 [文档](./index.html), 或者 检出 [示例的源码](https://github.com/marmelab/react-admin/tree/master/examples/demo) 参考示例的使用方法.

## 小展身手

```jsx
// in app.js
import React from 'react';
import { render } from 'react-dom';
import { Admin, Resource } from 'react-admin';
import simpleRestProvider from 'ra-data-simple-rest';

import { PostList, PostEdit, PostCreate, PostIcon } from './posts';

render(
    <Admin dataProvider={simpleRestProvider('http://localhost:3000')}>
        <Resource name="posts" list={PostList} edit={PostEdit} create={PostCreate} icon={PostIcon}/>
    </Admin>,
    document.getElementById('root')
);
```

这个 `<Resource>` 组件是配置组件,允许定义一些子组件的给不同的管理视图使用: `列表`, `编辑`, 和 `新建`. 这些组件使用 Material UI 和自定义的 react-admin 组件:

{% raw %}
```jsx
// in posts.js
import React from 'react';
import { List, Datagrid, Edit, Create, SimpleForm, DateField, TextField, EditButton, DisabledInput, TextInput, LongTextInput, DateInput } from 'react-admin';
import BookIcon from '@material-ui/icons/Book';
export const PostIcon = BookIcon;

export const PostList = (props) => (
    <List {...props}>
        <Datagrid>
            <TextField source="id" />
            <TextField source="title" />
            <DateField source="published_at" />
            <TextField source="average_note" />
            <TextField source="views" />
            <EditButton basePath="/posts" />
        </Datagrid>
    </List>
);

const PostTitle = ({ record }) => {
    return <span>Post {record ? `"${record.title}"` : ''}</span>;
};

export const PostEdit = (props) => (
    <Edit title={<PostTitle />} {...props}>
        <SimpleForm>
            <DisabledInput source="id" />
            <TextInput source="title" />
            <TextInput source="teaser" options={{ multiLine: true }} />
            <LongTextInput source="body" />
            <DateInput label="Publication date" source="published_at" />
            <TextInput source="average_note" />
            <DisabledInput label="Nb views" source="views" />
        </SimpleForm>
    </Edit>
);

export const PostCreate = (props) => (
    <Create title="Create a Post" {...props}>
        <SimpleForm>
            <TextInput source="title" />
            <TextInput source="teaser" options={{ multiLine: true }} />
            <LongTextInput source="body" />
            <TextInput label="Publication date" source="published_at" />
            <TextInput source="average_note" />
        </SimpleForm>
    </Create>
);
```
{% endraw %}

## 可以兼容我的 API 吗?

当然可以.

React-admin 使用一种适配器,近似一个叫 *数据提供器(Data Providers)* 的概念. 可以参考已有提供器来设计你的API, 或者你也能写自己的数据提供器来请求一个已有的API. 写一个自定义的数据提供器几个小时就能搞定.

![数据提供器架构图](./img/data-provider.png)

详情请看 [数据提供器文档](./DataProviders.md) .

## 自备全套工具,可按需裁剪

React-admin 设计为一个构建在[material-ui](http://www.material-ui.com/#/)之上的松耦合 React 组件库, 此外控制器函数是按照 Redux 的方式实现. 非常容易用你自己的替换掉一部分 react-admin 的功能, 比如: 用自定义的数据列表, GraphQL 代替 REST, 或者 bootstrap 代替 Material Design.

## 贡献

欢迎 Pull requests [GitHub repository](https://github.com/marmelab/react-admin). 尝试延续已有文件的编码风格, 包含单元测试和文档. 准备一次彻底的代码评审, 和等待代码合并的耐心 - 这是开源的流程.

你可以这样跑一个例子:

```sh
make run
```

然后浏览器打开 [http://localhost:8080/](http://localhost:8080/).

如果你想贡献文档, 安装 jekyll, 然后执行

```sh
make doc
```

然后浏览器打开 [http://localhost:4000/](http://localhost:4000/)

你可以这样跑单元测试

```sh
make test
```

如果你把 react-admin 作为一项依赖, 并且你想尝试修改react-admin, 建议这样处理:

```sh
# 在 myapp 里
# 从GitHub安装 react-admin 到另一个目录
$ cd ..
$ git clone git@github.com:marmelab/react-admin.git && cd react-admin && make install
# 通过软链接的形式,把你的 node_modules/react-admin 链接到刚才checkout的react-admin
$ cd ../myapp
$ npm link ../react-admin
# 回到 checkout, 修改react的版本,软连接到你app的react
$ cd ../react-admin
$ npm link ../myapp/node_modules/react
$ make watch
# 新起一个终端, 回到你的app, 正常启动
$ cd ../myapp
$ npm run
```

## 版权

React-admin 以 [MIT Licence](https://github.com/marmelab/react-admin/blob/master/LICENSE.md) 授权, 由 [marmelab](http://marmelab.com) 赞助和支持.

## 捐赠

这是一个免费使用的库, 不限商业化. 如果你想要回馈, 请谈论它, 帮助新人, 或者贡献代码. 最好的回馈方式是 **慈善捐赠**. 我们推荐 [Doctors Without Borders](http://www.doctorswithoutborders.org/).
