### Install typescript
```shell
npm i -g typescript
```

创建 `.ts` 文件

### Primitive types
```typescript
let myName: string = 'xiao';
let myNumber: number = 3;
let myBoolean: boolean = true;
```

### Custom type
```ts
type Person = {
	name: string
	age: number
}

let person1: Person = {
	name: 'xiao',
	age: 18,
}
```

### Nested Type and Optional Type
```ts
type Address = {
	street: string
	country: string
}

type Person = {
	name: string
	age: number
	address?: Address
}

```

### array type
```ts

let ages: number[] = [100, 101]
// or
let people: Array<Person> = [person1, person2]
```

### throw error
```ts
function completeOrder(orderId: number) {

	const order = orderQueue.find(order => order.id === orderId)
	// if order is undefined
	if (!order) {
		throw new Error(`${orderId} was not found in the orderQueue`)
	}
	order.status = "completed"
	return order
}
```

### union

```ts
// like options
type UserRole = "guest" | "member" | "admin"

let userRole: UserRole = "member"
```

### type narrowing

```ts
function getPizzaDetail(identifier: string | number) {
	if (typeof(identifier) == "string") {}
}
```

### type function return
```ts
function fetchUserDetails(username: string): void {
}

function fetchUserDetails(username: string):  Order| undefined {
	return 
}


function fetchUserDetails(username: string): User {
	return user
}
```

### any type
不做检查, 最好不要用

## Utility Type
### partical type
```ts
type User = {
	id: number
	username: string
	role: "member" | "contributor" | "admin"
}

type UpdatedUser = Partial<User>
```

### Omit Type

```ts
// remove id from User Type
Omit<User, 'id' | 'username'> 
```

# Generics
多态使用
```ts
// 可以接受所有的类型
function getLastItem<T>(array: T[]) {
	return array[array.length - 1]
}
```

```ts
function addToArray<T>(array: T[], item: T): T[] {
	array.push(item)
	return array
}
// 调用的时候, 也可以使用generic, 控制输入的对象
addToArray<Order>(orderQueue, { id: nextOrderId++, pizza: menu[2], status: "done" })
```