## Blog Post

Another learning application to hammer home some concepts related to react, redux, thunk, axios. Application comes from "Modern React with Redux" course from Udemy. Here are som notes

#### Some libraries we are using

- Redux
  - the redux library
- react-redux
  - integration layer between react and redux
- axios
  - helps us make network requests
- redux-thunk
  - middleware to help us make requests in a redux application

#### General Data Loading with Redux

- Components are generally responsible for fetching data they need by calling an action creator
  - Component gets rendered onto the screen
  - Component's 'componentDidMount' lifecycle method gets called
  - We call action creator from 'componentDidMount'
- Action creators are responsible for making API requests
  - Action creator runs code to make an API request
  - API responds with data
  - Action creator returns an 'action' with the fetched data on the 'payload' property
- We get fetched data into a component by generating new state in our redux store, then getitng that into our component through mapStateToProps
  - Some reducer sees the action, returns the data off the 'payload'
  - Because we generated some new state object, redux/react-redux cause our React app to be rerendered

#### Error: "use custom missleware for async actions"

- What's wrong with 'fetchPosts'?
  - Actions creators must return plain JS objects with a type property - we are not..
  - By the time our action gets to a reducer, we won't have fetched our data
- Synchronous action creator
  - Instantly returns an action with data ready to go
- Asynchronous action creator
  - Takes some amount of time for it to get its data ready to go
- Middleware as it relates to redux
  - Function that gets called with every action we dispatch
  - Has the ability to STOP, MODIFY, or otherwise mess around with actions
  - Tons of open source middleware exist
  - Most popular use of middleware is for dealing with async actions
  - We are going to use a middleware called 'Redux-Thunk' to solve our async issues

#### Rules of Reducers

- Must return any value besides 'undefined'
- Produces 'state', or data to be used inside of your app using only previous state and the action (reducers are pure)
- Must not return reach 'out of itself' to decide what value to return
- Must not mutate its input 'state' argument

##### State v1 + Action => Reducer -> State v2

#### Updating the state

- Removing an element from an array

  - BAD: state.pop()
  - GOOD: state.filter(element => element !== 'hi')

- Adding an element to an array

  - BAD: state.push('hi')
  - GOOD: [...state, 'hi']

- Replacing an element in an array

  - BAD: state[0] = 'hi'
  - GOOD: state.map(el => === 'hi' ? 'bye' : el)

- Updating a property in an object

  - BAD: state.name = 'Sam'
  - GOOD: { ...state, name: 'Sam'}

- Adding a property to an object

  - BAD: state.age = 30
  - GOOD: {...state, age: 30}

- Removing a property from an object
  - BAD: delete state.name
  - GOOD: { ...state, age: undefined } also \_.omit(state, 'age')

#### Memoizing Functions

- Issue: overfetching users. Making 10 requests to API when we should onlyl be making 1
- Solution: Memoization.
- \_.memoize( func, [resolver]) from the lodash libary (npm i --save lodash)
  - Creates a funciton that memoizes the result of func.

```javascript
func getUser(number){
    fetch(number);
    return "Request made"
}
const memoizedGetUser = _.memoize(getUser())

memoizedGetUser(3) // getUser is called with a request of 3
memoizedGetUser(3) // no request is made becuase it is MEMOIZED
memoizedGetUser(3) // still no request is made
memoizedGetUser(4) // a request is made of 4 because it is new
```

#### The memoized solution in this project

Can be found in actions/index.js

```javascript
export const fetchUser = id => dispatch => _fetchUser(id, dispatch);

const _fetchUser = _.memoize(async (id, dispatch) => {
	const response = await jsonPlaceholder.get(`/users/${id}`);

	dispatch({ type: "FETCH_USER", payload: response.data });
});
```

#### An alternate solution to memoization

fetchPostsAndUser()

- Call 'fetchPosts'
- Get list of posts
- Find all unique userId's from list of posts
- Iterate over unique userId's
- Call 'fetchUser' with each userId
