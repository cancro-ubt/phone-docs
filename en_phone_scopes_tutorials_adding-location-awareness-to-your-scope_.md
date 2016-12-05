





Ubuntu has a solid location stack, allowing users to select which applications
have access to the device location. This also applies to scopes and is very
easy to add to your code. In this short tutorial, you are going to learn how
to bring location awareness to your scope.

For this example, we are going to use a default scope template. Let’s start by
opening QtCreator and [create a new scope project](/scopes/tutorials/scope-development-procedures/) using the HTTP + JSON API template.

## Location settings

The first change we are going to make is in the
[<scope>.ini.in](http://bazaar.launchpad.net/~davidc3/ubuntu-sdk-tutorials/scope-tutorial-location-may2015/view/head:/src/data/settings-for-scopes-v2.ini.in) file. Its role is to declare some metadata for the scope and
relationships between the scope and the Dash (UI colors, name, etc.)

Open **src/data/<scope>.ini.in** in the editor and add the following
declaration to its ScopeConfig section, or copy the content of the [examplefile](http://bazaar.launchpad.net/~davidc3/ubuntu-sdk-tutorials/scope-tutorial-location-may2015/view/head:/src/data/settings-for-scopes-v2.ini.in):

8

`LocationDataNeeded = true`

This will automatically add a setting to your scope, allowing the user to
toggle location data access. Note that it will be enabled by default. If you
run the scope at this point, you should see a new “Settings” entry in the
header, with an “Enable location data” checkbox.

![](/static/devportal_uploaded/f860dad7-81bd-4396-868c-b50b2f7d8b6a-cms_page_media/144/scope-location-0.png)

![](/static/devportal_uploaded/fad54a0c-d176-4023-a538-8acfc49d4bd0-cms_page_media/144/scope-location-1.png)

## Location data

The template we are using is querying openweathermap.org to get current
weather. We are going to change it to use the user city and country, instead
of the hardcoded “London” query done by default.

When you are requesting location data, here is what the scopes API is giving
you access to :

#### Coordinates

  * altitude
  * latitude
  * longitude
  * horizontal_accuracy
  * vertical_accuracy

#### Toponyms and codes

  * city
  * region_name
  * country_name
  * area_code
  * country_code
  * region_code
  * zip_postal_code

Have a look at the [API documentation](https://developer.ubuntu.com/api/scopes/cpp/sdk-14.10/unity.scopes.Location/) to dive in all the details. As you will
see, each location data element has an equivalent has__element_() function to
ensure its availability.

### Changing the query

For the rest of the tutorial, you only need to edit
[src/query.cpp](http://bazaar.launchpad.net/~davidc3/ubuntu-sdk-tutorials/scope-tutorial-location-may2015/view/head:/src/query.cpp).

First, we are going to add a new include to get the search metadata associated
to each query:

9

`#include <unity/scopes/SearchMetadata.h>`

Then, in the Try part of _Query::run()_, we can request location data from
this metadata object. Reproduce the following lines or copy the content of the
[example file](http://bazaar.launchpad.net/~davidc3/ubuntu-sdk-tutorials/scope-tutorial-location-may2015/view/head:/src/query.cpp):

87

88

89

90

91

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

`// A string to store the location name for the openweathermap query`

`std::string place;`

`// Access search metadata`

`auto metadata = search_metadata();`

`// Check for location data`

`if` `(metadata.has_location()) {`

` ``auto location = metadata.location();`

` ``// Check for city and country`

` ``if` `(location.has_city() && location.has_country_name()) {`

` ``// Create the "city country" string`

` ``place = location.city() + ``" "` `+ location.country_name();`

` ``}`

`}`

`// Fallback to a hardcoded location`

`if` `(place.empty()) {`

` ``place = ``"London"``;`

`}`

Now, we just need to use the place variable in our default query:

117

118

119

120

121

122

123

`if` `(query_string.empty()) {`

` ``// If the string is empty, get the current location weather`

` ``current = client_.weather(place);`

`} ``else` `{`

` ``// otherwise, get the current weather for the search string`

` ``current = client_.weather(query_string);`

`}`

**That’s it!** The scope should now be able to surface weather data for your location when you open it.

![](/static/devportal_uploaded/7eee7ca9-7a11-46b3-be44-1d880391a38b-cms_page_media/144/scope-location-2.png)

## Next steps

Now that you have seen how to add more context to scope queries, you should
have a look at the [Settings tutorial](/scopes/tutorials/adding-settings-to-your-scope/) to give users more freedom to customize your scope and adapt it
to their needs.





