# Navigation

The navigation in XEO V4 is done through the Story/Scene paradigm as explained in the [New Concepts](New_Concepts) page. There are two ways to navigate to new scenes using the API
- Adding a new scene to the story directly
- Executing an existing script (there are some variants to this)

## Creating a new scene and adding it to the story

Best start with an example (assuming you the context is a bean that extends the ApplicationBaseBean):

```java
    Scene newScene = new Scene( "path/viewer.xvw" );
    renderScene( newScene );
```

And what if you need to pass some parameters to the viewer/bean?

```java
	Scene newScene = new Scene( "path/viewer.xvw" );
    MyBean bean = (MyBean) newScene.getBean();
    bean.setParam1("someValue");
    bean.setParam2("someOtherValue");
    renderScene( newScene );
```


## Executing a script

The other option when navigation is executing a Script. Scripts are basically a series of steps that culminate in the creation of Scene. For default scripts you can execute them directly, customize their parameters or extend them, let's see examples of them (again in the context of an ApplicationBaseBean).

##### Executing a script directly
```java
OpenEditViewerScript script = new OpenEditViewerScript( SOME_BOUI );
executeScript( script );
```

#### Customizing a script

Each default script provides a built-in "builder" method which provies a fluent interface for all the parameters it can receive.

```java
OpenEditViewerScript script = OpenEditViewerScript
.builder( SOME_BOUI )
.viewer( "path/to/viewer.xvw" ).build();
executeScript( script );

```

#### Extending a script

Every step of the default scripts is a protected method that you can override to your need, like in the following example:

```java
public class MyOpenViewer extends OpenEditViewerScript{

    public MyOpenViewer( long boui ) {
        super( boui );
    }

    @Override
    protected String getViewToOpen( boObject object ) {
        return "myViewer.xvw";
    }
    
    @Override
    protected void initializeBean(ApplicationBaseBean bean, boobject object){
    	super.initializeBean(bean,object);
        object.setDisabled();
    }

}
```
And then
```java
MyOpenViewer script = new MyOpenViewer( SOME_BOUI );
executeScript( script );
```

Check the javadocs for each of the scripts to check which methods you can override