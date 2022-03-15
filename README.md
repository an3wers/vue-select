# vue-select component

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

## About component

### The component takes as input
1. Request in json format (for example: https://jsonplaceholder.typicode.com/users)
2. The field to be taken from the data structure (for example, "name")
3. Placeholder - string.

```
<app-select
    placeholder="Выбрать пользователя..."
    :data="users"
    param="name"
></app-select>
```

### The output is an object

```
{id: 1, value: Foo, selected: true }
```