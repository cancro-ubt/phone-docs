





In this recipe you will learn how to write a currency converter app for Ubuntu
on the phone. You will be using several components from the Ubuntu QML
toolkit: _i18n_, _units_, _ItemStyle_ for theming, _Label_,
_ActivityIndicator_, _Popover_, _Button_, _TextField_, _ListItems.Header_ and
_ListItems.Standard_

The application will show you how to use the QML declarative language to
create a functional user interface and its logic, and to communicate through
the network and fetch data from a remote source on the Internet.

In practical terms, you will be writing an application that performs currency
conversion between two selected currencies. The rates are fetched using the
European Central Bank’s API. Currencies can be changed by pressing the buttons
and selecting the currency required from the list.

## Requirements

  * Ubuntu 14.04 or later – [get Ubuntu](http://www.ubuntu.com/download/desktop/)
  * The Ubuntu SDK – [install the Ubuntu SDK](/en/phone/platform/sdk/installing-the-sdk/)

## The tools

The focus of this tutorial will be on the Ubuntu UI toolkit preview and its
components, rather than on the tools. However, it is worth mentioning and
giving an overview of the tools you will be using:

### Development host

**Ubuntu 14.04** (or later) will be used as the host machine for development. At the end of this recipe you will have created a platform-agnostic QML app that can be run on the development host machine. Subjects such as cross-compiling for a different architecture and installation on a phone are more advanced topics that will be covered at a later date when the full Ubuntu SDK is released.

### Integrated Development Environment (IDE)

We will be writing declarative QML code, which does not need to be compiled to
be executed, so **you can use your favourite text editor** to write the actual
code, which will consist of a single QML file.

For this tutorial, **we recommend using Ubuntu SDK**. Ubuntu SDK is a powerful
IDE to develop applications based on the Qt framework.

### QML viewer

To start QML applications, either during the prototyping or final stages, we
will use **Ubuntu SDK** and the Ctrl+R shortcut.

However, as an alternative for quick app viewing with QML Scene, it is worth
noting that you can also use **QML Scene** without Ubuntu SDK. QML Scene is a
command-line application that interprets and runs QML code.

To run a QML application with QML Scene, open up a terminal with the
Ctrl+Alt+T key combination, and execute the qmlscene command, followed by the
path to the application:

`$ qmlscene /path/to/application.qml`

[Learn more about QML Scene](http://qt-project.org/doc/qt-5.0/qtquick/qtquick-qmlscene.html)

## Getting started

To start Ubuntu SDK, simply open the **Dash**, start typing “**ubuntu sdk**“,
and click on the Ubuntu SDK icon that appears on the search results.

![](/static/devportal_uploaded/6f550612-b39f-43a8-a0cf-4254b9e6c830-cms_page_media/269/ubuntu-sdk-dash.jpg)

Next stop: putting our developer hat on.

### The main view

We’ll start off with a minimum QML canvas with our first Ubuntu component: a
label inside the main view.

  1. In Ubuntu SDK, press Ctrl+N to create a new project
  2. Select the** Projects > Ubuntu > App with Simple UI** template and click **Choose…**
  3. Give the project **CurrencyConverter** as a **Name**. You can leave the **Create in:** field as the default and then click **Next**.
  4. You can optionally set up a revision control system such as Bazaar in the final step, but that’s outside the scope of this tutorial. Click on **Finish**.
  5. Replace the **Column** component and all of its children, and replace them with the **Page** as shown below, and then **save** it with Ctrl+S:
    import QtQuick 2.0
    import Ubuntu.Components 1.1
    /*!
        \brief MainView with a Label and Button elements.
    */
    MainView {
        id: root
        // objectName for functional testing purposes (autopilot-qt5)
        objectName: "mainView"
        // Note! applicationName needs to match the "name" field of the click manifest
        applicationName: "currencyconverter.yourname"
        /*
         This property enables the application to change orientation
         when the device is rotated. The default is false.
        */
        //automaticOrientation: true
        // Removes the old toolbar and enables new features of the new header.
        useDeprecatedToolbar: false
        width: units.gu(100)
        height: units.gu(75)
        property real margins: units.gu(2)
        property real buttonWidth: units.gu(9)
        Page {
            title: i18n.tr("Currency Converter")
        }
    }

Try to run it now to see the results:

  1. Inside Ubuntu SDK, press the Ctrl+R key combination. It is a shortcut to the **Build > Run** menu entry

Or alternatively, from the terminal:

  1. Open a terminal with Ctrl+Alt+T
  2. Run the following command:

`qmlscene ~/CurrencyConverter/main.qml`

![](/static/devportal_uploaded/a48c7a0f-312e-40ae-b82d-65e258cba4f6-cms_page_media/269/converter_0.png)

Hooray! Your first Ubuntu app for the phone is up and running. Nothing very
exciting yet, but notice how simple it was to bootstrap it. You can close your
app for now.

Now starting from the top of the file, let’s go through the code.

1

2

`import QtQuick 2.0`

`import Ubuntu.Components 1.1`

Every QML document consists of two parts: an imports section and an object
declaration section. First of all we import the QML types and components that
we need, specifying the namespace and its version. In our case, we import the
built-in QML and Ubuntu types and components.

We now move on to declaring our objects. In QML, a user interface is specified
as a tree of objects with properties. JavaScript can be embedded as a
scripting language in QML as well, but we’ll see this later on.

    MainView {
        id: root
        // objectName for functional testing purposes (autopilot-qt5)
        objectName: "mainView"
        // Note! applicationName needs to match the "name" field of the click manifest
        applicationName: "currencyconverter.yourname"
        /*
         This property enables the application to change orientation
         when the device is rotated. The default is false.
        */
        //automaticOrientation: true
        // Removes the old toolbar and enables new features of the new header.
        useDeprecatedToolbar: false
        width: units.gu(100)
        height: units.gu(75)
        property real margins: units.gu(2)
        property real buttonWidth: units.gu(9)
        Page {
            title: i18n.tr("Currency Converter")
        }
    }

Secondly, we create a [MainView](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.MainView/), the most essential SDK component, which
acts as the root container for our application. It also provides the standard
toolbar and [Header](http://design.ubuntu.com/apps/building-blocks/header).

With a syntax similar to JSON, we define its
[properties](http://doc.qt.io/qt-5/qtqml-syntax-propertybinding.html) by
giving it an id we can refer it to (_root_), and then we define some visual
properties (_width_, _height_, _color_). Notice how in QML properties are
bound to values with the ‘_property: value_‘ syntax. We also define a custom
property called _margins_, of [type](http://doc.qt.io/qt-5/qtqml-typesystem-basictypes.html) [real](http://doc.qt.io/qt-5/qml-real.html) (a number with
decimal point). Don’t worry about the _buttonWidth_ property for now, we’ll
use it later on. The rest of the properties available in the MainView we leave
at their default values by not declaring them.

Notice how we specify units as _units.gu_. These are **grid units**, which we
are going to talk about in a minute. For now, you can consider them as a form-
factor-agnostic way to specify measurements. They return a pixel value that’s
dependent on the device the application is running on.

Inside our main view, we add a child [Page](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.Page/), which will contain the rest of our
components as well as provide a title. We title text to the page, ensuring it
is enclosed with the _i18n.tr()_ function, which will make it translatable.

### Resolution independence

A key feature of the Ubuntu user interface toolkit is the ability to scale to
all form factors in a world defined by users with multiple devices. The
approach taken has been to define a new unit type, the grid unit (gu in
short). Grid units translate to a pixel value depending on the type of screen
and device the application is running on. Here are some examples:

**Device**
**Conversion**

Most laptops

1 gu = 8 px

Retina laptops

1 gu = 16 px

Smart phones

1 gu = 18 px

[Learn more about resolution independence](https://developer.ubuntu.com/api/apps/qml/current/UbuntuUserInterfaceToolkit.resolution-independence/)[Learn moreabout resolution independence](https://developer.ubuntu.com/api/apps/qml/current/UbuntuUserInterfaceToolkit.resolution-independence/)

### Internationalization

As part of the Ubuntu philosophy, internationalization and native language
support is a key feature of the Ubuntu toolkit. We’ve chosen _gettext_ as the
most ubiquitous Free Software internationalization technology, which we’ve
implemented in QML through the family of _i18n.tr()_ functions.

## Fetching and converting currencies

Now we will start adding the logic to our app, which will mean getting the
currency and rates data and doing the actual conversion.

Start by adding the following code around line 33 **before the Page’s closing
brace**. We will mostly be appending code in all subsequent steps, but any
snippet will be contained inside our root MainView. So when you append code,
make sure it is still before the MainView’s closing brace at the end of the
file.

    ListModel {
        id: currencies
        ListElement {
            currency: "EUR"
            rate: 1.0
        }
        function getCurrency(idx) {
            return (idx >= 0 && idx < count) ? get(idx).currency: ""
        }
        function getRate(idx) {
            return (idx >= 0 && idx < count) ? get(idx).rate: 0.0
        }
    }

What we are doing here is to use _currencies_ as a
[ListModel](http://doc.qt.io/qt-5/qml-qtqml-models-listmodel.html) object that
will contain a list of items consisting of _currency_ and _rate_ pairs. The
_currencies_ ListModel will be used as a source for the view elements that
will display the data. We will be fetching the actual data from the Euro
foreign exchange reference rates from the European Central Bank. As such, the
Euro itself is not defined there, so we’ll pre-populate our list with the EUR
currency, with a reference rate of 1.0.

The function statements in currencies illustrate another powerful feature of
QML: integration with JavaScript. The two JavaScript functions are used as
glue code to retrieve a currency or rate from an index. They are required as
currencies may not be loaded when component property bindings use them for the
first time. But do not worry much about their function. For now it’s just
important to remember that you can transparently [integrate JavaScript code inyour QML documents](http://doc.qt.io/qt-5/qtqml-javascript-expressions.html).

Now we’ll fetch the actual data with a QtQuick object to load XML data into a
model: the [integrate JavaScript code in your QMLdocuments](http://doc.qt.io/qt-5/qml-qtquick-xmllistmodel-xmllistmodel.html).
To use it, we add an additional import statement at the top of the file, so
that it looks like:

    import QtQuick 2.0
    import QtQuick.XmlListModel 2.0
    import Ubuntu.Components 1.1

And then around line 49, add the actual rate exchange fetcher code:

    XmlListModel {
        id: ratesFetcher
        source: "http://www.ecb.int/stats/eurofxref/eurofxref-daily.xml"
        namespaceDeclarations: "declare namespace gesmes='http://www.gesmes.org/xml/2002-08-01';"
                               +"declare default element namespace 'http://www.ecb.int/vocabulary/2002-08-01/eurofxref';"
        query: "/gesmes:Envelope/Cube/Cube/Cube"
        onStatusChanged: {
            if (status === XmlListModel.Ready) {
                for (var i = 0; i < count; i++)
                    currencies.append({"currency": get(i).currency, "rate": parseFloat(get(i).rate)})
            }
        }
        XmlRole { name: "currency"; query: "@currency/string()" }
        XmlRole { name: "rate"; query: "@rate/string()" }
    }

The relevant properties are _source_, to indicate the URL where the data will
be fetched from; _query_, to specify an absolute
[XPath](https://developer.mozilla.org/en-US/docs/XPath) query to use as the
base query for creating model items from the _XmlRoles_ below; and
_namespaceDeclarations_ as the namespace declarations to be used in the XPath
queries.

The _onStatusChanged_ **signal handler** demonstrates another combination of
versatile features: the signal and handler system together with JavaScript.
Each QML property has got a _<property>Changed_ signal and its corresponding
on_<property>Changed_ signal handler. In this case, the _StatusChanged_ signal
will be emitted to notify of any changes of the status property, and we define
a handler to append all the currency/rate items to the _currencies_ ListModel
once _ratesFetcher_ has finished loading the data.

In summary, _ratesFetcher_ will be populated with currency/rate items, which
will then be appended to _currencies_.

It is worth mentioning that in most cases we’d be able to use a single
XmlListModel as the data source, but in our case we use it as an intermediate
container. We need to modify the data to add the EUR currency, and we put the
result in the _currencies_ ListModel.

Notice how network access happens transparently so that you as a developer
don’t have to even think about it!

Around line 66, let’s add an [ActivityIndicator](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.ActivityIndicator/) component to show
activity while the rates are being fetched:

    ActivityIndicator {
        objectName: "activityIndicator"
        anchors.right: parent.right
        running: ratesFetcher.status === XmlListModel.Loading
    }

We anchor it to the right of its parent (_root_) and it will show activity
until the rates data has been fetched.

And finally, around line 32 (above and outside of the _Page_), we add the
_convert_ JavaScript function that will perform the actual currency
conversions:

        function convert(from, fromRateIndex, toRateIndex) {
            var fromRate = currencies.getRate(fromRateIndex);
            if (from.length <= 0 || fromRate <= 0.0)
                return "";
            return currencies.getRate(toRateIndex) * (parseFloat(from) / fromRate);
        }

## Choosing currencies

At this point we’ve added all the backend code and we move on to user
interaction. We’ll start off with creating a new
[Component](http://doc.qt.io/qt-5/qml-qtqml-component.html), a reusable block
that is created by combining other components and objects.

Let’s first append two import statements at the top of the file, underneath
the other import statements:

    import Ubuntu.Components.ListItems 0.1
    import Ubuntu.Components.Popups 0.1

And then add the following code around line 79:

    Component {
        id: currencySelector
        Popover {
            Column {
                anchors {
                    top: parent.top
                    left: parent.left
                    right: parent.right
                }
                height: pageLayout.height
                Header {
                    id: header
                    text: i18n.tr("Select currency")
                }
                ListView {
                    clip: true
                    width: parent.width
                    height: parent.height - header.height
                    model: currencies
                    delegate: Standard {
                        objectName: "popoverCurrencySelector"
                        text: currency
                        onClicked: {
                            caller.currencyIndex = index
                            caller.input.update()
                            hide()
                        }
                    }
                }
            }
        }
    }

At this point, if you run the app, you will not yet see any visible changes,
so don’t worry if all you see is an empty rectangle.

What we’ve done is to create the currency selector, based on a [Popover](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.Popups.Popover/
) and a standard Qt Quick [ListView](http://doc.qt.io/qt-5/qml-qtquick-listview.html). The ListView will display the data from the _currencies_
ListMode. Notice how the Column object wraps the [Header](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.ListItems.Header/) and the
list view to arrange them vertically, and how each item in the list view will
be a [Standard](https://developer.ubuntu.com/api/apps/qml/current/Ubuntu.Components.ListItems.Standard/) list item component.

The popover will show the selection of currencies. Upon selection, the popover
will be hidden (see _onClicked_ signal) and the caller’s data is updated. We
assume that the caller has _currencyIndex_ and _input_ properties, and that
_input_ is an item with an _update()_ function.

## Arranging the UI

Up until now we’ve been setting up the backend and building blocks for our
currency converter app. Let’s move on to the final step and the fun bit,
putting it all together and seeing the result!

Add the final snippet of code around line 110:

            Column {
                id: pageLayout
                anchors {
                    fill: parent
                    margins: root.margins
                }
                spacing: units.gu(1)
                Row {
                    spacing: units.gu(1)
                    Button {
                        id: selectorFrom
                        objectName: "selectorFrom"
                        property int currencyIndex: 0
                        property TextField input: inputFrom
                        text: currencies.getCurrency(currencyIndex)
                        onClicked: PopupUtils.open(currencySelector, selectorFrom)
                    }
                    TextField {
                        id: inputFrom
                        objectName: "inputFrom"
                        errorHighlight: false
                        validator: DoubleValidator {notation: DoubleValidator.StandardNotation}
                        width: pageLayout.width - 2 * root.margins - root.buttonWidth
                        height: units.gu(5)
                        font.pixelSize: FontUtils.sizeToPixels("medium")
                        text: '0.0'
                        onTextChanged: {
                            if (activeFocus) {
                                inputTo.text = convert(inputFrom.text, selectorFrom.currencyIndex, selectorTo.currencyIndex)
                            }
                        }
                        function update() {
                            text = convert(inputTo.text, selectorTo.currencyIndex, selectorFrom.currencyIndex)
                        }
                    }
                }
                Row {
                    spacing: units.gu(1)
                    Button {
                        id: selectorTo
                        objectName: "selectorTo"
                        property int currencyIndex: 1
                        property TextField input: inputTo
                        text: currencies.getCurrency(currencyIndex)
                        onClicked: PopupUtils.open(currencySelector, selectorTo)
                    }
                    TextField {
                        id: inputTo
                        objectName: "inputTo"
                        errorHighlight: false
                        validator: DoubleValidator {notation: DoubleValidator.StandardNotation}
                        width: pageLayout.width - 2 * root.margins - root.buttonWidth
                        height: units.gu(5)
                        font.pixelSize: FontUtils.sizeToPixels("medium")
                        text: '0.0'
                        onTextChanged: {
                            if (activeFocus) {
                                inputFrom.text = convert(inputTo.text, selectorTo.currencyIndex, selectorFrom.currencyIndex)
                            }
                        }
                        function update() {
                            text = convert(inputFrom.text, selectorFrom.currencyIndex, selectorTo.currencyIndex)
                        }
                    }
                }
                Button {
                    id: clearBtn
                    objectName: "clearBtn"
                    text: i18n.tr("Clear")
                    width: units.gu(12)
                    onClicked: {
                        inputTo.text = '0.0';
                        inputFrom.text = '0.0';
                    }
                }
            }

It’s a piece of code that’s longer than previous snippets, but it is pretty
simple and there is not much new in terms of syntax. What we’re doing is
arranging the visual components to provide user interaction within the _root_
area and defining signal handlers.

Notice how we use the _onClicked_ signal handlers to define what will happen
when the user clicks on the currency selectors (i.e. the pop ups are opened),
the _onTextChanged_ handler to call the _convert_() function defined earlier
to do conversions as we type, and we define the _update()_ function the list
view items from the _currencySelector_ component defined earlier expect.

We are using a Column and two Rows to set up the layout, and each row contains
a currency selector button and a text field to display or input the currency
conversion values. We’ve also added a button below them to clear both text
fields at once. Here’s a mockup to illustrate the layout:

![](/static/devportal_uploaded/a6c12d22-2d1a-4d95-b28c-71c861bee5a1-cms_page_media/269/PageLayout_2.png)

## Lo and behold

So that’s it! Now we can lay back and enjoy our creation. Just press the
Ctrl+R shortcut within Ubuntu SDK, and behold the fully functional and slick
currency converter you’ve just written with a few lines of code.

![](/static/devportal_uploaded/b1dec9a8-a67f-402a-9d20-38b201f53427-cms_page_media/269/converter_1.png)

![](/static/devportal_uploaded/7fa6751d-3f86-4fd1-8d63-b4d0d25e5a9c-cms_page_media/269/converter_2.png)

## Test it!

Now that the application is running, don't forget about tests! The [qualitypage for qml applications](/en/phone/platform/quality/) has you covered. Learn
about writing tests for every level of the testing pyramid by using the
application you just built.

## Conclusion

You’ve just learned how to write a form-factor-independent Ubuntu application
for the phone. In doing that, you’ve been exercising and combining the power
of technologies such as QML, Javascript and a variety of Ubuntu components, to
produce an app with a cohesive, crisp and clean Ubuntu look.

You’ll surely have noticed the vast array of possibilities these technologies
open up, so it’s now up to you: help us testing the toolkit preview, write
your own apps and give us your feedback to make Ubuntu be in the next billion
phones!

## Learn more

If this tutorial has started whetting your appetite, you should definitely
check out the **Component Showcase** app that comes with the Ubuntu QML
toolkit preview. With it, you’ll be able to see all of the Ubuntu components
in action and look at their code to learn how to use it in your apps.

![](/static/devportal_uploaded/b30c9376-defa-4bbc-813f-e45c4469f095-cms_page_media/269/ui_gallery.png)

If you want to study the Component Showcase code:

  1. **Start Ubuntu SDK** by pressing the Ubuntu button in the Launcher. That will bring up the **Dash**.
  2. Start typing _ubuntu sdk_ and click on the Ubuntu SDK icon.
  3. In Ubuntu SDK, then press the Ctrl+O key combination to open the file selection dialog.
  4. **Select** the file _/usr/lib/ubuntu-ui-toolkit/examples/ubuntu-ui-toolkit-gallery/ubuntu-ui-toolkit-gallery.qml_ and click on **Open**.
  5. To run the code, you can select the Tools > External > Qt Quick > Qt Quick 2 Preview (qmlscene) menu entry.

Alternatively, if you only want to run the Component Showcase:

  1. Open the Dash
  2. Type “toolkit gallery” and double click on the “Ubuntu Toolkit Gallery” result that appears to run it

### Reference

  * [Code for this tutorial](http://bazaar.launchpad.net/~ubuntu-sdk-tutorials-dev/ubuntu-sdk-tutorials/trunk/files/head:/getting-started/CurrencyConverter/) (use bzr branch lp:ubuntu-sdk-tutorials to get a local copy)
  * [Writing tests for Currency Converter](/en/phone/platform/quality/)
  * [Ubuntu UI Toolkit API documentation](https://developer.ubuntu.com/api/apps/qml/current/)
  * [Qt Quick documentation](http://qt-project.org/doc/qt-5.0/qtquick/qtquick-index.html)
  * [Getting Started Programming with Qt Quick](http://qt-project.org/doc/qt-5.0/qtdoc/gettingstartedqml.html)
  * [Syntax of the QML language](http://qt-project.org/doc/qt-5.0/qtqml/qtqml-index.html#syntax-of-the-qml-language)
  * [Integrating JavaScript and QML](http://qt-project.org/doc/qt-5.0/qtquick/qtquick-usecase-integratingjs.html)

## Questions?

If you’ve got any questions on this tutorial, or on the technologies that it
uses, just [get in touch with our App Developer Community]()!





