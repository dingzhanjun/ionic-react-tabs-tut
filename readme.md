# Ionic React (Beta) Tabs: Step By Step

Working with the new ionic cli generating an app with tabs and eventually a login page

Ionic & React Components used in this example:

- [IonTabs Documentation](https://ionicframework.com/docs/api/tabs)
- [IonBackButton](https://ionicframework.com/docs/api/back-button)
- [React Router Documentation](https://reacttraining.com/react-router/web/guides/quick-start)

## What It Will Look Like
<p align="center">
<img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab-1.png" width="33%">
<img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab1-detail.png" width="33%">
</p>

<p align="center">

  <img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab-2.png" width="33%">
</div>

## Getting Started

use the ionic cli to build your app, make sure you specify react and we are going to use the tab starter as our baseline and then move some things around to get the desired results. 

>See blog post for more detailed instructions on getting started [Blog Post Here](https://ionicframework.com/blog/ionic-cli-v5-brings-react-beta-support/)

Enter into console, and when prompted select `tabs` as the starter template
```
$ ionic start myApp --type=react
```
```
? Starter template: tabs
```

### Housecleaning
So lets clean up some of this and create a more structured starting point.

Create a new file called `TabRoot.tsx` and copy everything from inside of the `IonApp` element in `App.tsx` over to the new component. When you are done, `App.tsx` should look like this

```tsx

// FILE: App.tsx

const App: React.SFC = () => (
  <Router>
    <Route exact path="/" render={() => <Redirect to="/tab1"/>} />
    <div className="App">
      <IonApp>

      </IonApp>
    </div>
  </Router>
);
```
Remove this line
```tsx

// FILE: App.tsx

<Route exact path="/" render={() => <Redirect to="/tab1"/>} />
```
Then add the new default `Route` to point to the `TabRoot` component we just built
```tsx

// FILE: App.tsx

const App: React.SFC = () => (
  <Router>
    <div className="App">
      <IonApp>
         <Route path="/" component={TabRoot} />
      </IonApp>
    </div>
  </Router>
);
```
And `TabRoot.tsx` should look like this after pasting the code we cut from `App.tsx`.

>Please note I have removed the imports to save space when looking at the code. I also have reduced the number of tabs from three to two, I believe that is sufficcient to make my point.

>Please note that the documentation regarding how IonTabs work in React doesnt appear to be correct

```tsx

// FILE: TabRoot.tsx

interface IAppProps {}

const TabRoot: React.FC<IAppProps> = props => {
  return (
    <IonPage id="main">
      <IonTabs>
        <IonRouterOutlet>
          <Route path="/:tab(tab1)" component={Tab1} exact={true} />
          <Route path="/:tab(tab2)" component={Tab2} />
        </IonRouterOutlet>
        <IonTabBar slot="bottom">
          <IonTabButton tab="tab1" href="/tab1">
            <IonIcon name="flash" />
            <IonLabel>Tab One</IonLabel>
          </IonTabButton>
          <IonTabButton tab="tab2" href="/tab2">
            <IonIcon name="apps" />
            <IonLabel>Tab Two</IonLabel>
          </IonTabButton>
        </IonTabBar>
      </IonTabs>
    </IonPage>
  );
};

export default TabRoot;
```
Now the application is set up such that the default route is to render the `TabRoot` component, but we need to tell the component which tab to render and we want it to be `Tab1`
```html

// FILE: TabRoot.tsx

<IonRouterOutlet>
    <Route path="/:tab(tab1)" component={Tab1} exact={true} />
    <Route path="/:tab(tab2)" component={Tab2} />
    <Route path="/" render={() => <Redirect to="/tab1" />} /> 
</IonRouterOutlet>
```
### Why Bother?
Having all of the default routing based around tabs at the route level of the application can become problematic as the application gets more complex. As you will see in the later sections when the app has to check for authenticated user and protected routes, this setup will be beneficial

### Cleanup Tab1
There is alot of noise in `Tab1` so lets make it look like `Tab2`, copy contents from `Tab2` into `Tab1`
```tsx

// FILE: Tab1.tsx

import React from 'react';
import { IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/react';

const Tab1: React.SFC = () => {
  return (
    <>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Tab One</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent />
    </>
  );
};

export default Tab1;
```
## Navigating to Detail Pages
Lets just duplicate the file `Tab1.tsx` and rename it `Tab1Detail.tsx`... clean it up so it looks like this when you are done.
```tsx

// FILE: Tab1Detail.tsx

import React from 'react';
import { IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/react';

const Tab1Detail: React.SFC = () => {
  return (
    <>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Tab One Detail</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent />
    </>
  );
};

export default Tab1Detail;
```
Add button in the `IonContent` section of `Tab1`; we will use that button to navigate to the detail page `Tab1Detail` that we just created.
```tsx

// FILE: Tab.tsx

<IonContent>
    <IonButton
        expand="full"
        style={{ margin: "14" }}
        onClick={e => {
            e.preventDefault();
            props.history.push("/tab1-detail");
        }}
    > NEXT PAGE</IonButton>
</IonContent>
```
So a few problems when you make this change in `Tab1.tsx`, the first one is
>Where is the props.history coming from?

We can use react-router `withRouter` to get the `history` object passed along as a property to the component since the component was being rendered by the `Router`. So lets make the following changes to the files.
```tsx

// FILE: Tab1.tsx

// add the import..
import { withRouter } from "react-router";
```
Then add parameter, and for now we will specify the type as `any`
```tsx

// FILE: Tab1.tsx

const Tab1: React.SFC<any> = (props) => {
```
Finally we need to add the actual route we want to navigate to `/:tab(tab1-detail)` to the `Router` element in `TabRoot`, So add the new route.
```tsx

// FILE: TabRoot.tsx

<IonRouterOutlet>
    <Route path="/:tab(tab1)" component={Tab1} />
    <Route path="/:tab(tab1-detail)" component={Tab1Detail} />
    <Route path="/:tab(tab2)" component={Tab2} />
    <Route path="/" render={() => <Redirect to="/tab1" />} /> 
</IonRouterOutlet>
```
Now to go back, we need to first add the `IonBackButton` component to the toolbar on the `Tab1Detail` page, right above the `<IonTitle>`.
```tsx

// FILE: Tab1Detail.tsx

<IonButtons slot="start">
  <IonBackButton
      text=""
      defaultHref="/"
      onClick={() => props.history.replace("/tab1")}
      goBack={() => {}}
  />
</IonButtons>
<IonTitle>Tab One Detail</IonTitle>
```
>Known Issue: As of now, the `defaultHref` is not working so I had to respond to the `onCLick` event to get this to work.

As you can see we are using the history propery again to go back to the previous component so we need to add the `withRouter` and properly specify the parameters for the component.
```tsx

// FILE: Tab1Detail.tsx

import { withRouter } from "react-router";        // <== NEW

const Tab1Detail: React.SFC<any> = (props) => {   // <== NEW
  return (
    <>
      <IonHeader>
        <IonToolbar>
          <IonButtons slot="start">
            <IonBackButton
              text=""
              defaultHref="/tab1"
              onClick={ ()=> props.history.replace("/tab1")}  // <== NEW
              goBack={() => {}}
            />
          </IonButtons>
          <IonTitle>Tab One Detail</IonTitle>
        </IonToolbar>
      </IonHeader>
      <IonContent />
    </>
  );
};

export default withRouter(Tab1Detail);  // <== NEW
```
