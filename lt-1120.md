<https://github.com/lbryio/lbry-android/issues/1120>

"remove spaces and other invalid characters in direct resolve..."

I cannot derive from the desktop code how the search query is transformed into the urls that are resolved so as to replicate here and present a PR, but if anyone knows better:

The resolution to get the featured item is launched from the LighthouseSearchTask in class SearchFragment, where the original query (eg "naomi brockwell") is transformed into ("lbry://naomi brockwell) via class LbryUri. While we are passing a single url, the task launching the resolution, ResolveTask, is ready to receive more than one. I added a second one ("lbry://@naomibrockwell") and then the expected featured item appeared (see screenshot). Obviously, with more than one url, more than one result may be received, and right now only one is considered for display.

![featuredAndroid](https://user-images.githubusercontent.com/8489669/118944876-7c582c00-b944-11eb-9615-0c5e36d03611.png)

By the way, search results _do_ include @naomibrockwell for query "naomi brockwell", which is why that item is duplicated in the screenshot, so for regular results this derivation is done in the server. Note that this duplication does not happen in desktop (screenshot below), so it really pays to know how this is handled there to work it out in Android. 

![featuredDesktop](https://user-images.githubusercontent.com/8489669/118944907-837f3a00-b944-11eb-9a39-d1071aed450d.png)

LbryUri in lbry-redux and url.go in url.go ("matching" LbryUri.java in lbry-android) _may_ be a good place too look at, but maybe the transformation is done somewhere else.

Hope it is of some help.
