
Proxy有点像是python中的dunner方法，或者getter， setter方法，用来增加或者扩展对象的基本行为。

```javascript

const gameSettings = { date: "2019-12-30"};
const gameSetitngsProxy = new Proxy(gameSettings, {
	// 取对象中的元素，如果key存在，则返回value， 如果不存在，则返回fanxiao
	get: (o, property) => {
		return property in o ? o[property] : "No such key"
	}, 

	set: (o, property, value) => {
		if (property === 'difficulty' && !['easy', 'medium', 'hard'].includes(value.toLowerCase())) {
			throw new Error('Difficulty is invalid');
		}
		o[property] = value
	}
});

console.log(gameSettingsProxy.date); // 2019-12-30
console.log(gameSettingsProxy.difficulty); // No such key 


gameSettingsProxy.difficulty = "easy" // 成功

```

Vue使用了proxy做数据的双向绑定