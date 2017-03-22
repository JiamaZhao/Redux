--actions 用户的行为
--components 放的是组件，决定如何显示，从props获取数据，修改数据的方式，从redux获取state,手写
--container  放的也是组件，决定如何获取数据，从Redux获取state, 修改数据的方式，向redux发送actions， redux生成
--reducer    store里面来分发用户的行为
--index.html 模板文件
--server.js  用这个来构建
--webpack    webpack打包配置文件


const visibleTodoList = connect(
        mapStateToProps,   // 这个方法会订阅store,每次store更新的时候都更新ui，  这个参数也可以省略，这閪的话store更新就不会引起ui的更新
        mapDispatchToProps  // 定义了哪些用户的操作应该当作Action传给store,它可以是一个函数，也可以是一个对象，函数返回的也是对象
    )(TodoList);  

const mapStateToProps = (state) => {
    return {
        todos: getVisibleTodos(state.todos, state.visibilityFilter) // getvisibleTodos()可以由state计算出todos这个Props的值，下面有个getVisibleTodos()方法的例子
    }
}

const getVisibleTodos= () => {
    switch() {
        case 'SHOW_ALL':
         return todos;
        case 'SHOW_COMPLEMED'
         return todos.filter(t => t.completed)
        case 'SHOW_ACTIVE':
         return todos.filter(t => !t.completed)
        default 
         throw new Error('Unknow filter:' + filter)
    }
}

// mapStateToProps还有第二个参数，代表容器组件的props
//<FilterLink filter = 'SHOW_ALL'>
//   ALL
//</FILTERLink>
const mapStateToProps = (state, ownProps) => {
    return {
        active: ownProps.filter === state.visibilityFilter;  // 使用容器组件作为参数后，如果组件的参数发生变化ui组件也会重新渲染
    }
}



const visibleTodoList = connect(
        mapStateToProps,   
        mapDispatchToProps  // 定义了哪些用户的操作应该当作Action传给store,它可以是一个函数，也可以是一个对象，函数返回的也是对象
    )(TodoList); 

mapDispatchToProps =(
    dispatch,
    ownProps
    ) => {
    return {
        onClick: () => {
            dispatch({
                type: 'SET_VISIBILITY_FILTER'
                filter: ownProps.filter
            });
        }
    }
}

//或者写成对象的形式
const mapDispatchToProps = {
  onClick: (filter) => {
    type: 'SET_VISIBILITY_FILTER',
    filter: filter
  };
}


//provider快速拿到state,App的所有子组件都可以拿到state了
import {Provider} from 'react-redux'
import {createStore} from 'redux'
import todoApp from './reducer'
import App from './components/App'

let store = createStore(todoApp)

render(
    <Provider store={store}>
        <App/>
    </Provider>
    );

//计数器，两个组件属性,value要从state获取数据,onIncreaseClick()要向外发出Action

class Counter extends component {
    render() {
        const { value, onIncreaseClick } = this.props
        return (
            <div>
                <span>{value}</span>
                <button onClick={onIncreaseClick}>Increase</button>
            </div>
        )
    }
}

function mapStateToProps(state) {
    return {
        value: state.count
    }
}

function mapDispatchToProps(dispatch) {
    return {
        onIncreaseClick:() => dispatch(increaseAction)
    }
}

//action Creator
const increaseAction = {type: 'increase'}

const App = connect(
    mapStateToProps,
    mapDispatchToProps
)(counter);


//下面定义组件的Reducer
function counter (state= {count: 0}, action) {
    const count state.count
    switch (action.type) {
        case 'increase': 
            return {count =count + 1 }
        default:
            return state;        
    }
}






































