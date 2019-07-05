# Redux简介
> 一般来说，当需要根据角色判断使用方式、与服务器大量交互 (例如使用 WebSocket)、视图需要从多个来源获取数据，也就是说在交互复杂、多数据源时；或者从组件的角度考虑，如果需要组件的状态广播等时需要使用。
## Redux 的设计思想

A) Web 应用是一个状态机，视图与状态是一一对应；B) 所有的状态，保存在一个对象里面。

可以简单将 Redux 理解为是 JavaScript 的状态容器：

- 应用中所有的状态都是以一个对象树的形式存储在一个单一的 store 中；
- 当你想要改变应用的中的状态时，你就要 dispatch 一个 action，这也是唯一的改变 state 的方法；
- 通过编写 reducer 来维护状态，返回新的 state，不直接修改原来数据；

## Redux的工作流

首先由view dispatch拦截action，然后执行对应reducer并更新到store中，最终views会根据store数据的改变执行界面的刷新渲染操作。

同时，作为一款应用状态管理框架，为了让应用的状态管理不再错综复杂，使用Redux时应遵循三大基本原则，否则应用程序很容易出现难以察觉的问题。这三大原则包括：

- **单一数据源**
整个应用的State被存储在一个状态树中，且只存在于唯一的Store中。
- **State是只读的**
对于Redux来说，任何时候都不能直接修改state，唯一改变state的方法就是通过触发action来间接的修改。而这一看似繁琐的状态修改方式实际上反映了Rudux状态管理流程的核心思想，并因此保证了大型应用中状态的有效管理。
- **应用状态的改变通过纯函数来完成**
Redux使用纯函数方式来执行状态的修改，Action表明了修改状态值的意图，而真正执行状态修改的则是Reducer。且Reducer必须是一个纯函数，当Reducer接收到Action时，Action并不能直接修改State的值，而是通过创建一个新的状态对象来返回修改的状态。

## Redux中的基本概念

**1.Store**

Store 就是保存数据的地方，可以把它看成一个容器，整个应用只能有一个 Store ； Redux 通过提供的 createStore() 这个函数来生成 Store 。

	import { createStore } from 'redux';
	const store = createStore(fn);

其中 **createStore()** 函数接受另一个函数作为参数，返回新生成的 Store 对象。

**2.State**

Store 对象包含所有数据，如果想得到某个时点的数据，就要对 Store 生成快照，这种时点的数据集合，就叫做 State 。

当前时刻的 State 可以通过 **store.getState()** 拿到

	import { createStore } from 'redux';
	const store = createStore(fn);
	
	const state = store.getState();

Redux 规定，state 和 view 一一对应，一个 State 对应一个 View，只要 State 相同，View 就相同；反之亦然。

** 3.Action**

如上所述，State 的变化，会导致 View 的变化；但是，用户接触不到 State，只能接触到 View 。所以，State 的变化必须是 View 导致的，Action 就是 View 发出的通知，表示 State 应该要发生变化了。

Action 是一个对象，其中的 type 属性是必须的，表示 Action 的名称，其它属性可以自由设置，社区有一个 规范 可以参考。

	const action = {
		type: 'ADD_TODO',
		payload: 'Learn Redux'
	};

上面代码中，Action 的名称是 **ADD_TODO**，它携带的信息是字符串 Learn Redux 。

可以这样理解，Action 描述当前发生的事情，改变 State 的唯一办法，就是使用 Action，它会运送数据到 Store 。

**4. Action Creator**

View 要发送多少种消息，就会有多少种 Action，如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator。

	const ADD_TODO = '添加 TODO';
	
	function addTodo(text) {
		return {
			type: ADD_TODO,
			text
		}
	}
	
	const action = addTodo('Learn Redux');

上面代码中，**addTodo()** 函数就是一个 Action Creator 。

**5. store.dispatch()**

**store.dispatch()** 是 View 发出 Action 的唯一方法。

	import { createStore } from 'redux';
	const store = createStore(fn);
	
	store.dispatch({
		type: 'ADD_TODO',
		payload: 'Learn Redux'
	});

上面代码中，**store.dispatch** 接受一个 Action 对象作为参数，将它发送出去。

**6. Reducer**

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化，这种 State 的计算过程就叫做 Reducer 。

**Reducer 是一个函数，它接受 Action 和当前 State 作为参数，返回一个新的 State** 。

	const defaultState = 0;
	const reducer = (state = defaultState, action) => {
		switch (action.type) {
		case 'ADD':
			return state + action.payload;
		default:
			return state;
		}
	};
	
	const state = reducer(1, {
		type: 'ADD',
		payload: 2
	});



