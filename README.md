# UIComponentDescriptor2XML

This script is used to convert UIComponentDescriptor to XML file

warrning: it has many bugs =_=

bugs:
1.  it can't resolve array in propertiesFactory, like: `"columns":[_Test_DataGridColumn9_c(), _Test_DataGridColumn10_c()]`, it will just ignore it, you need to follow the functions to restore the tags and add them as children to this tag

============================

After decompile a Flex project, you may found it difficult to read some .as files. 
That's because some decompilers can not decompile the byte code to MXML file completely, which may cause the following code:

```ActionScript
new UIComponentDescriptor({
    "type":Canvas,
    "id":"mainCanvas",
    "stylesFactory":function ():void {
        this.borderStyle = "none";
        color = 0xFFFFFF;
    },
    "propertiesFactory":function ():Object {
        return ({
            "label":"main",
            "percentWidth":100,
            "percentHeight":100,
            "visible":true,
            "childDescriptors":[
                new UIComponentDescriptor({
                    "type":CheckBox,
                    "id":"check",
                    "events":{"click":"__check_click"},
                    "stylesFactory":function ():void {
                        this.fontSize = 12;
                    },
                    "propertiesFactory":function ():Object {
                        return ({
                            "x":39,
                            "y":8,
                            "label":"check",
                            "selected":false
                        });
                    }
                })
            ]
        });
    }
});
```

With this script, it can be convert to:

```XML
<?xml version="1.0" ?>
<mx:Canvas borderStyle="none" color="0xFFFFFF" id="mainCanvas" label="main" percentHeight="100" percentWidth="100" visible="true">
    <mx:CheckBox click="__check_click(event)" fontSize="12" id="check" label="check" selected="false" x="39" y="8"/>
</mx:Canvas>
```

Use config file to setup, it can be written like this:

```
#this is the config file, lines that begin with '#' will be treated as comments
#write original file path and target file path like these examples
#@<D:\\a.as><D:\\b.xml>
#@<./a.as><C:\\b.xml>
#or you can ignore target path, if you do that, it will create a file in the same dir of original file
#@<D:\\a.as>
#and the target file will be D:\\a.as.xml

@<D:\\a.as>
@<D:\\c.as> <D:\\ccc.xml>

#to configure prefix of xml tags, you can write like this
#$<canvas><s:>
#it means each <canvas /> tag will become <s:canvas />

$<MyCanvas><UI:>

#to configure default prefix of other xml tags, you can write like this
#&<mx:>
#it means each tag that doesn't have matching prefix will use this as prefix, like : <mx:Label />

&<mx:>
```
