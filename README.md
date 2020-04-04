# React docs by Ajay

Small docs for React.

## React routing
```
import React from 'react';
import { BrowserRouter, Route } from 'react-router-dom'; // import BrowserRouter & Route from react-router-dom
import './App.css';
import '../node_modules/bootstrap/dist/js/bootstrap.min.js'; // import bootstarp.min.js from node_modules
import '../node_modules/font-awesome/css/font-awesome.min.css';  
import Home  from './components/home';
import Nav from './components/nav';
import Footer from './components/footer';
import CreatePost from './components/createPost';
import Show from './components/show';
import EditPost from './components/editPost';


function App() {
  return (
    <div className="App">
        {/* here is how react routing works */}
      <BrowserRouter> 
        <Nav/>
        <Route path="/" component={Home} exact />
        <Route path="/create" component={CreatePost} /> 
        {/* pass id & slug in urls */}
        <Route path="/show/:id" component={Show} /> 
        <Route path="/edit/:id" component={EditPost} /> 
        <Footer/>
      </BrowserRouter>
    </div>
  );
}

export default App;

```

## Pass params in url, i.e id, slug
```
import { Link } from 'react-router-dom'; //first of all import Link from react-router-dom

<Link  to={`show/${PostData[key].slug}`} className="btn btn-primary btn-sm" >Read More ></Link> //then pass the slug or id with route like that

<Route path="/show/:id" component={Show} /> // the route should have /:id
```

## Redirect in React

```
this.props.history.push('/')
```

## Axios call - get, post, patch, delete

### Post call -
```
import React, { Component } from 'react';
import axios from 'axios'; //first import axios
import CKEditor from '@ckeditor/ckeditor5-react';
import ClassicEditor from '@ckeditor/ckeditor5-build-classic';

class CreatePost extends Component {
    constructor(props) {
        super(props);

        this.onChangeTitle = this.onChangeTitle.bind(this); // bind this to onChangeTitle function -> so this can be get access in onChangeTitle function.
        this.onChangeCategory = this.onChangeCategory.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);

        this.state = { 
            // place states
            title: '',
            category: '',
            body: '',

            newCategory: '',
         }
    }

    // onChange update states
    onChange(e){
        this.setState({
            body: e.target.value
        });
    }

    // onChange update states
    onChangeTitle(e){
        this.setState({
            title: e.target.value
        });
    }

    // onChange update states
    onChangeCategory(e){
        this.setState({
            newCategory: e.target.value
        });
    }

    handleSubmit(event) {
        event.preventDefault();

        let postData = [this.state.title, this.state.newCategory, this.state.body]; //create postData array
        console.log(postData); //console.log(postData);

        // make axios post call
        axios.post('http://localhost:8000/api/posts',{
            title: postData[0],
            category: postData[1],
            body: postData[2],
        })
        .then(res =>{
            this.props.history.push('/')// then redirect it to home
            console.log(res)
        })
    }

    render() { 
        let postData = this.state; //access this.state in postData
        return ( 
            <div>
                <div className="container mt-5 mb-5">
                    <h1>Create Post.</h1>

                    <div className="row">
                        <div className="col-md-8">
                            {/* on click submit, handleSubmit function will get call */}
                            <form onSubmit={this.handleSubmit}> 
                                <div className="form-group">
                                    <label >Title: </label>
                                    {/* make reactive input */}
                                    <input type="text"  value={postData.title || ''} onChange={this.onChangeTitle} className="form-control"  placeholder="Enter Title" /> 
                                </div>
                                <div className='form-group'>
                                    <label>Category: </label>
                                    {/* make reactive select */}
                                    <select  value={postData.newCategory || ''} onChange={this.onChangeCategory} className="custom-select" >
                                        { 
                                            Object.keys(postData.category).map(function (key) {
                                                return ( 
                                                    <option key={key}>{postData.category[key].category}</option>
                                                )
                                            })
                                        }
                                    </select>
                                </div>
                                <div className="form-group">
                                   <label> Body:</label>
                                    <CKEditor
                                        editor={ ClassicEditor }
                                        // on change,  setState - body
                                        onChange={ ( event, editor ) => {
                                            const data = editor.getData();
                                            this.setState({
                                                body: data
                                            });
                                        } }
                                    />
                                </div>
                                <div className="form-group">
                                    <button type="submit" className="btn btn-primary">Publish</button>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
         );
    }

    // On page refresh componentDidMount get call
    componentDidMount(){
        this.getCategories();// getCategories
    }

    // axios get call
    getCategories(){
        axios.get( 'http://127.0.0.1:8000/api/category' )
        .then(res=>{
            this.setState({
                category: res.data
            })
            // console.log(res.data)
        })
    }
}
 
export default CreatePost;

```
## Pass Props in react
```
 render(props){      // add props to function
        return(
            <div className={`alert alert-${this.props.color}`} role="alert"> // then bind the props wherever you want.
                This is a primary alert—check it out!
            </div>
        )
    }
    
    // In your html pass props value 
     <BaseAlert color="danger" />
```
