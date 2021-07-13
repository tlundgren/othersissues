<https://github.com/ProtonVPN/android-app/issues/62>

“Allow blacklisting servers in profiles”


I think that a possible solution could be to display an additional drop down list, with the servers of the selected country, whenever the user selects fastest, or random, in the profile screen. This new list would be used to blacklist servers. Later, when the user connects using that profile, the app would disregardd the blacklisted servers.

The new list would display the same servers that the only list present today displays, only without the fastest and random servers, and would have checkboxes (or another mechanism) for the user to select servers. It would be placed in layout content_profile.xml after inputLayoutServer, and have its visibility toggled as indicated earlier. Whatever the UI implementation, however, one way this functionality could be added is:

**1.** Have class ServerWrapper hold a list of String representing the id's of the blacklisted servers (provided by the user through the UI), just in the same way as it now holds a String representing the id of the selected server.
```
data class ServerWrapper(
    // ...
    val serverId: String?,
    val blackListServerIds: List<String>,
    // ...
```

**2.** Have implementations of ServerDeliver (ie ServerManager) disregard the list of blacklisted servers in the server wrapper in method "getServer(wrapper: ServerWrapper): Server?". More specificallly...

**3.** In method "getRandomServer(country: VpnCountry): Server?", add a parameter server wrapper and replace...
```
val online = whiteList.filter(Server::online)
// with...
val online = country.serverList.filter { server->
    server.online && !wrapper.blackListServerIds.contains(server.serverId)
}
```

**4.** In method "getBestScoreServer(country: VpnCountry): Server?", add a parameter server wrapper and replace...
```
getBestScoreServer(country.serverList)
// with...
getBestScoreServer(country.serverList.filter { server ->
    !wrapper.blackListServerIds.contains(server.serverId)
})
```
