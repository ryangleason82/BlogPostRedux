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
