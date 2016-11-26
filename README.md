# Nova Apollo (Clean Version)

Some background:

- **Telescope** was originally launched as a pure-Meteor Reddit clone.
- In early 2016, a new React-based version codenamed **Nova** was created. 
- In late 2016, Nova's data layer was ported to [Apollo](http://apollostack.com).

This is a **clean** implementation of this Apollo version, in the sense that only core features are presented here. 

You can view this codebase as underlying the development framework that supports all other Nova features such as posts, comments, etc.

You can use this framework to **develop any type of Apollo-powered Meteor & React app**.

### Install

Clone this repo locally, then:

```
npm install
meteor
```

And open `http://localhost:3000/movies` in your browser.

### Features

Core Nova features include:

##### GraphQL Schema Generation

Nova will automatically generate GraphQL schemas for your collections based on their [SimpleSchema](https://github.com/aldeed/meteor-simple-schema) JSON schema. 

This prevents you from having to specify your schema twice in two different formats. Although please note that this feature is completely optional, and that you can also specify your schema manually if you prefer. 

##### Easy Data Loading

Nova features a set of data loading helpers to make loading Apollo data easier, `withList` (to load a list of documents) and `withSingle` (to load a single document). 

For example, here's how you would use `withList` to pass a `results` prop containing all movies to your `MoviesList` component:

```js
const listOptions = {
  collection: Movies,
  queryName: 'moviesListQuery',
  fragmentName: 'myFragment',
  fragment: fragment,
};

export default withList(listOptions)(MoviesList);
```

You can pass a fragment to control what data is loaded for each document.

##### Automated Forms

Nova will also use your schema to generate client-side forms and handle their submission via the appropriate Apollo mutation. 

For example, here's how you would display a form to edit a single movie:

```js
<NovaForm 
  collection={Movies} 
  documentId={props.documentId}
  queryName="moviesListQuery"
  showRemove={true}
/>
```

The `queryName` option tells NovaForm which query should be automatically updated once the operation is done, while the `showRemove` option add a “Delete Movie” button to the form. 

Note that NovaForm will also take care of loading the document to edit, if it's not already loaded in the client store. 

##### Schema-based Security & Validation

All of Nova's security and validation is based on your collection's schema. For each field of your schema, you can define the following functions:

- `viewableIf`
- `insertableIf`
- `editableIf`

They all take the current user as argument (and optionally the document being affected) and check if the user can perform the given action. 

##### Groups & Permissions

Nova features a simple system to handle user groups and permissions. For example, here's how you would define that all users can create new movies and edit/remove their own, but only admins can edit or remove other user's movies:

```js
const defaultActions = [
  "movies.new",
  "movies.edit.own",
  "movies.remove.own",
];
Users.groups.default.can(defaultActions);

const adminActions = [
  "movies.edit.all",
  "movies.remove.all"
];
Users.groups.admins.can(adminActions);
```

You can then reference these actions in your mutation checks. 

##### Other Features

Out of the box, Nova also includes many other time-saving features, such as:

- Internationalization
- Server-side rendering
- Utilities for theming and extending components
- Email and email templates handling
- Preset boilerplate mutations

And of course, using Nova also means you get access to all the non-core Nova packages, such as `nova:posts`, `nova:comments`, `nova:newsletter`, `nova:search` etc.

### Next Steps

We're looking for contributors! If you're interested in this project, come say hello in the [Nova Slack chatroom](http://slack.telescopeapp.org).
