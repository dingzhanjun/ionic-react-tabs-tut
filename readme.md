Ionic & React Components used in this example:

- [IonTabs Documentation](https://ionicframework.com/docs/api/tabs)
- [IonBackButton](https://ionicframework.com/docs/api/back-button)
- [React Router Documentation](https://reacttraining.com/react-router/web/guides/quick-start)

## What It Will Look Like

<table>
<tr>
<td style="margin:0px; padding:0px;">
<img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab-1.png"></td>
<td style="margin:0; padding:0;">
<img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab1-detail.png"></td>
<td  style="margin:0; padding:0;">
<img src="https://raw.githubusercontent.com/aaronksaunders/ionic-react-tabs-tut/master/public/screen-shots/tab-2.png"></td>
</tr>
</table>


## Getting Started

use the ionic cli to build your app, make sure you specify react and we are going to use the tab starter as our baseline and then move some things around to get the desired results. 

>See blog post for more detailed instructions on getting started [Blog Post Here](https://ionicframework.com/blog/ionic-cli-v5-brings-react-beta-support/)

Enter into console, and when prompted select `tabs` as the starter template
https://gist.github.com/2a2ace89ae5b2f75ca44dae17c2aa34a

### Housecleaning
So lets clean up some of this and create a more structured starting point.

Create a new file called `TabRoot.tsx` and copy everything from inside of the `IonApp` element in `App.tsx` over to the new component. When you are done, `App.tsx` should look like this
https://gist.github.com/2e4f1e69965b50083cec8a19d05ee2da

Remove this line
https://gist.github.com/c9200ade1e8c92e88fbffcff5129fb55

Then add the new default `Route` to point to the `TabRoot` component we just built
https://gist.github.com/bc0643e4eecc9d6d19fe4d456c70208e

And `TabRoot.tsx` should look like this after pasting the code we cut from `App.tsx`.

>Please note I have removed the imports to save space when looking at the code. I also have reduced the number of tabs from three to two, I believe that is sufficcient to make my point.

>Please note that the documentation regarding how IonTabs work in React doesnt appear to be correct

https://gist.github.com/784eee2d9ac4d4f78614e8b01f1df69c

Now the application is set up such that the default route is to render the `TabRoot` component, but we need to tell the component which tab to render and we want it to be `Tab1`
https://gist.github.com/14934f296cbedbc0d500b92c7236e36e

### Why Bother?
Having all of the default routing based around tabs at the route level of the application can become problematic as the application gets more complex. As you will see in the later sections when the app has to check for authenticated user and protected routes, this setup will be beneficial

### Cleanup Tab1
There is alot of noise in `Tab1` so lets make it look like `Tab2`, copy contents from `Tab2` into `Tab1`
https://gist.github.com/c96a0857421624e0c77354045624fec3

## Navigating to Detail Pages
Lets just duplicate the file `Tab1.tsx` and rename it `Tab1Detail.tsx`... clean it up so it looks like this when you are done.
https://gist.github.com/25da98879a001ca1de0f539d9a4c4bc1

Add button in the `IonContent` section of `Tab1`; we will use that button to navigate to the detail page `Tab1Detail` that we just created.
https://gist.github.com/7f0f14db78589b06270539664462f485

So a few problems when you make this change in `Tab1.tsx`, the first one is
>Where is the props.history coming from?

We can use react-router `withRouter` to get the `history` object passed along as a property to the component since the component was being rendered by the `Router`. So lets make the following changes to the files.
https://gist.github.com/cb8a7334244f85436ae5f7ffd121e77d

Then add parameter, and for now we will specify the type as `any`
https://gist.github.com/0fa0892d0f5a48af5efcd7b277efc6d4

Finally we need to add the actual route we want to navigate to `/:tab(tab1-detail)` to the `Router` element in `TabRoot`, So add the new route.
https://gist.github.com/2df3674024e0c300ceebd4fcd5410ef4

Now to go back, we need to first add the `IonBackButton` component to the toolbar on the `Tab1Detail` page, right above the `<IonTitle>`.
https://gist.github.com/ff8638c93d71a46341b2adb63ba1ad23

>Known Issue: As of now, the `defaultHref` is not working so I had to respond to the `onCLick` event to get this to work.

As you can see we are using the history propery again to go back to the previous component so we need to add the `withRouter` and properly specify the parameters for the component.
https://gist.github.com/35c4946fbe02456c4d383ac733a466c6

