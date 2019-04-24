---
layout: default
title: "授权(Authorization)"
---

# 授权(Authorization)

一些应用会要求对特定资源区分不同用户的访问权限.正如有许多不同的策略(单用户, 多用户或者多权限, 等等), react-admin 简单提供了钩子来执行你的授权相关的代码.

默认,一个react-admin应用不需要授权. 然后,如果需要的话,会依赖`授权提供器(authProvider)` 在 [鉴权](./Authentication.html) 里有章节介绍.

## 配置授权提供器

当一个组件需要检验用户的权限时,会调用 `授权提供器` 的 `查询授权权限(AUTH_GET_PERMISSIONS)` 类型查询.

下面时一个例子: 当调用权限校验时,通过 `授权提供器` 验证过后,存储用户的角色, 并返回结果:

{% raw %}
```jsx
// in src/authProvider.js
import { AUTH_LOGIN, AUTH_LOGOUT, AUTH_ERROR, AUTH_GET_PERMISSIONS } from 'react-admin';
import decodeJwt from 'jwt-decode';

export default (type, params) => {
    if (type === AUTH_LOGIN) {
        const { username, password } = params;
        const request = new Request('https://mydomain.com/authenticate', {
            method: 'POST',
            body: JSON.stringify({ username, password }),
            headers: new Headers({ 'Content-Type': 'application/json' }),
        })
        return fetch(request)
            .then(response => {
                if (response.status < 200 || response.status >= 300) {
                    throw new Error(response.statusText);
                }
                return response.json();
            })
            .then(({ token }) => {
                const decodedToken = decodeJwt(token);
                localStorage.setItem('token', token);
                localStorage.setItem('role', decodedToken.role);
            });
    }
    if (type === AUTH_LOGOUT) {
        localStorage.removeItem('token');
        localStorage.removeItem('role');
        return Promise.resolve();
    }
    if (type === AUTH_ERROR) {
        // ...
    }
    if (type === AUTH_CHECK) {
        return localStorage.getItem('token') ? Promise.resolve() : Promise.reject();
    }
    if (type === AUTH_GET_PERMISSIONS) {
        const role = localStorage.getItem('role');
        return role ? Promise.resolve(role) : Promise.reject();
    }
    return Promise.reject('Unknown method');
};
```
{% endraw %}

## 限制对特定资源或者视图的访问权限

 在 `管理(Admin)` 组件里, 可以限制对特定资源或者视图的访问权限. 首先, 你要指定一个函数作为 `管理(Admin)` 的唯一子节点. 该函数会在查询 `授权提供起(authProvider)` 返回的允许权限时被调用.

{% raw %}
```jsx
<Admin
    dataProvider={dataProvider}
    authProvider={authProvider}
>
    {permissions => [
        // Restrict access to the edit and remove views to admin only
        <Resource
            name="customers"
            list={VisitorList}
            edit={permissions === 'admin' ? VisitorEdit : null}
            icon={VisitorIcon}
        />,
        // Only include the categories resource for admin users
        permissions === 'admin'
            ? <Resource name="categories" list={CategoryList} edit={CategoryEdit} icon={CategoryIcon} />
            : null,
    ]}
</Admin>
```
{% endraw %}

注意这个函数返回的是 React 元素的数组. 要避免把他们封装到容器元素, 否则 `管理(Admin)` 不生效.

**提示** 要留意当完全排除掉一个资源(像上面例子里的 `categories` 资源)时, 对其他资源视图也完全不可见.

## 限制对特定字段和输入的访问控制

你可能只想对部分有特殊权限用户展示一些字段或者输入. 这些权限可以通过每次路由获取到,并提供给你的组件作为 `权限(permissions)` 属性.

每次路由都会调用 `授权提供器(authProvider)` 的 `查询授权权限(AUTH_GET_PERMISSIONS)` 类型的接口, 传入参数诸如当前的地址(location)和路由参数(route parameters). 由你决定返回什么在你的组件里检查, 比如用户角色, 等.

比如, 在 `新建(Create)` 视图里, 有一个 `简易表格(SimpleForm)` 和一个自定义 `菜单栏(Toolbar)`:

{% raw %}
```jsx
const UserCreateToolbar = ({ permissions, ...props }) =>
    <Toolbar {...props}>
        <SaveButton
            label="user.action.save_and_show"
            redirect="show"
            submitOnEnter={true}
        />
        {permissions === 'admin' &&
            <SaveButton
                label="user.action.save_and_add"
                redirect={false}
                submitOnEnter={false}
                variant="flat"
            />}
    </Toolbar>;

export const UserCreate = ({ permissions, ...props }) =>
    <Create {...props}>
        <SimpleForm
            toolbar={<UserCreateToolbar permissions={permissions} />}
            defaultValue={{ role: 'user' }}
        >
            <TextInput source="name" validate={[required()]} />
            {permissions === 'admin' &&
                <TextInput source="role" validate={[required()]} />}
        </SimpleForm>
    </Create>;
```
{% endraw %}

**提示** 注意 `权限(permissions)` 属性 是由自定义组件 `菜单栏(toolbar)` 传进来的.

也可以用在 `编辑(Edition)` 视图里的 `页签表单(TabbedForm)`, 这样就能完全隐藏其中的某个 `表单页签(FormTab)`:

{% raw %}
```jsx
export const UserEdit = ({ permissions, ...props }) =>
    <Edit title={<UserTitle />} {...props}>
        <TabbedForm defaultValue={{ role: 'user' }}>
            <FormTab label="user.form.summary">
                {permissions === 'admin' && <DisabledInput source="id" />}
                <TextInput source="name" validate={required()} />
            </FormTab>
            {permissions === 'admin' &&
                <FormTab label="user.form.security">
                    <TextInput source="role" validate={required()} />
                </FormTab>}
        </TabbedForm>
    </Edit>;
```
{% endraw %}

对 `列表(List)` 视图, `数据表格(DataGrid)`, `简易列表(SimpleList)` 和 `查询过滤器(Filter)` 组件也同样适用.

{% raw %}
```jsx
const UserFilter = ({ permissions, ...props }) =>
    <Filter {...props}>
        <TextInput
            label="user.list.search"
            source="q"
            alwaysOn
        />
        <TextInput source="name" />
        {permissions === 'admin' ? <TextInput source="role" /> : null}
    </Filter>;

export const UserList = ({ permissions, ...props }) =>
    <List
        {...props}
        filters={<UserFilter permissions={permissions} />}
        sort={{ field: 'name', order: 'ASC' }}
    >
        <Responsive
            small={
                <SimpleList
                    primaryText={record => record.name}
                    secondaryText={record =>
                        permissions === 'admin' ? record.role : null}
                />
            }
            medium={
                <Datagrid>
                    <TextField source="id" />
                    <TextField source="name" />
                    {permissions === 'admin' && <TextField source="role" />}
                    {permissions === 'admin' && <EditButton />}
                    <ShowButton />
                </Datagrid>
            }
        />
    </List>;
```
{% endraw %}

**提示** 留意 `权限(permissions)` 属性是如何传进自定义的 `查询过滤器(filters)` 组件.

## 限制对数据面板里特定内容的访问

[`数据面板(dashboard)`]('./Admin.md#dashboard) 组件也可以接收权限(permissions)属性:

{% raw %}
```jsx
// in src/Dashboard.js
import React from 'react';
import Card from '@material-ui/core/Card';
import CardContent from '@material-ui/core/CardContent';
import { Title } from 'react-admin';

export default ({ permissions }) => (
    <Card>
        <Title title="Dashboard" />
        <CardContent>Lorem ipsum sic dolor amet...</CardContent>
        {permissions === 'admin'
            ? <CardContent>Sensitive data</CardContent>
            : null
        }
    </Card>
);
```
{% endraw %}

## 限制对自定义页面内容的访问

你可以在[自定义页面](./Admin.md#customroutes)中校验用户权限. 要用到 `权限允许(WithPermissions)` 组件来完成. 会确保用户是鉴权过的, 并且调用 `授权提供器(authProvider)` 的 `查询授权权限(AUTH_GET_PERMISSIONS)` 类型和 `验证` It will ensure the user is authenticated then call the `authProvider` with the `AUTH_GET_PERMISSIONS` type and the `authParams` you specify:

{% raw %}
```jsx
// in src/MyPage.js
import React from 'react';
import Card from '@material-ui/core/Card';
import CardContent from '@material-ui/core/CardContent';
import { Title, WithPermissions } from 'react-admin';
import { withRouter } from 'react-router-dom';

const MyPage = ({ permissions }) => (
    <Card>
        <Title title="My custom page" />
        <CardContent>Lorem ipsum sic dolor amet...</CardContent>
        {permissions === 'admin'
            ? <CardContent>Sensitive data</CardContent>
            : null
        }
    </Card>
)
const MyPageWithPermissions = ({ location, match }) => (
    <WithPermissions
        authParams={{ key: match.path, params: route.params }}
        // location is not required but it will trigger a new permissions check if specified when it changes
        location={location}
        render={({ permissions }) => <MyPage permissions={permissions} /> }
    />
);

export default MyPageWithPermissions;

// in src/customRoutes.js
import React from 'react';
import { Route } from 'react-router-dom';
import Foo from './Foo';
import Bar from './Bar';
import Baz from './Baz';
import MyPageWithPermissions from './MyPage';

export default [
    <Route exact path="/foo" component={Foo} />,
    <Route exact path="/bar" component={Bar} />,
    <Route exact path="/baz" component={Baz} noLayout />,
    <Route exact path="/baz" component={MyPageWithPermissions} />,
];
```
{% endraw %}

## Restricting Access to Content in Custom Menu

What if you want to check the permissions inside a [custom menu](./Admin.md#menu) ? Much like getting permissions inside a custom page, you'll have to use the `WithPermissions` component:

{% raw %}
```jsx
// in src/myMenu.js
import React from 'react';
import { connect } from 'react-redux';
import { MenuItemLink, WithPermissions } from 'react-admin';

const Menu = ({ onMenuClick, logout }) => (
    <div>
        <MenuItemLink to="/posts" primaryText="Posts" onClick={onMenuClick} />
        <MenuItemLink to="/comments" primaryText="Comments" onClick={onMenuClick} />
        <WithPermissions
            render={({ permissions }) => (
                permissions === 'admin'
                    ? <MenuItemLink to="/custom-route" primaryText="Miscellaneous" onClick={onMenuClick} />
                    : null
            )}
        />
        {logout}
    </div>
);
```
{% endraw %}
