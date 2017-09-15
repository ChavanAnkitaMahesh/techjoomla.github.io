---
date: 2017-01-15
title: Introduction
description: API framework for Joomla
categories:
  - Joomla REST API
type: Document
---

# API framework for Joomla

## Introduction

com_api is a quick and easy way to add REST APIs to Joomla. Extendible via plugins, you an easily add support for more Joomla extensions. To get started, download the component and install the API plugins you need. Enable the plugins and you are ready to fetch your content via APIs

## Using APIs

To add additional resources to the API, plugins need to be created. Each plugin can provide multiple API resources. Plugins are a convenient way to group several resources. Eg: A single plugin could be created for Quick2Cart with separate resources for products, cart, checkout, orders etc.

## Plugin Terms

### app
An app is essentially a Joomla plugin. However the plugin itself does nothing more than load the resources it contains. So the app is mainly used to package the API plugin and to enable adding any API specific parameters. Each app will have one or more resources. 

### resource
Resources are files that have code to accept input and set the API output. You will usually have multiple resources in an app. A common use case is for an extension like Easysocial or Jomsocial to have a single app. The app contains resources for various objects like groups, events, photos, newsfeed etc. The resource will contain the methods `get()` `post()` `delete()` to perform CRUD operations on that type of object.

### key / token
The key is used to access authenticated resources. The admin section allows you to create keys. It's also possible to use the `/api/user/login` API to login using username and password and get a token in response.


## Calling resources

### API URLs
The URL to access any route is of the format - 

`/api/{plugin}/{resource}`

Tip : If your resource expects an `id` parameter in the URL, you can use `/api/{plugin}/{resource}/{id}` as the API url. Other querystring need to be sent as is.

To enable SEF URLs for endpoints, make sure you have created a Joomla menu of the type API > API Endpoint. If you create the menu using any other alias than `api` make sure you use the apppropriate slug in the endpoint. If you do not have SEF URLs enabled, use  the endpoint URL as index.php?option=com_api&app={app}&resource={resource}&format=raw.

### Authentication
The API token is used for authentication. The token needs to be passed via the Authorization header using the Bearer scheme eg: `Authorization: Bearer <token>`. Previous versions also allowed passing the token as a querystring variable with the name `key`. The querystring approach will be deprecated in the future version. It is possible for an app to make an entire resource or a specific HTTP method in a resource public

Note : Sometimes Apache does not pass on the Authorization header, in such cases send then token using the X-Authorization header i.e. `X-Authorization: Bearer {token}`

### Output Format
The default output format is JSON. However it's also possible to get XML output by setting the `Accept: application/xml` header.

## Overriding Output
If you wish to modify the 'envelope' of the response, you can copy the file `components/com_api/libraries/response/jsonresponse.php` into `templates/{your template}/json/api.php` and modify the structure of the output. A similar override is possible for XML as well. 