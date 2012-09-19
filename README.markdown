Why?
----

I needed a way to present a searchable list of items that come from a (likely) asynchronous source (such as querying a web backend.) There are many widgets to do this type of thing in web/JS applications, but Googling didn't turn up any pre-built widgets for iOS.

What?
-----

JCAutocompletingSearch allows you to present a search controller that executes queries via its delegate. One likely source of the results would a web backend. Given the asynchronous (and delayed) nature of these types of searches, JCAutocompletingSearch automatically shows a loading indicator when necessary as well as managing out-of-order responses which shouldn't be displayed.

The delegate is required to implement searching, a cancel button action, and result selected action. It can optionally implement methods controlling whether or not to perform a blank search on load, whether or not searches are performed synchronously (synchronous searches won't display the loading indicator cell), and giving the ability to present results in custom UITableView cells.

![Sample application screenshot](https://github.com/jcoleman/JCAutocompletingSearch/raw/master/screenshot.png "Screenshot of sample application on iPhone")

Example Code?
-------------

A working sample iOS Xcode project is available in the `Demo` directory.

Usage?
----

    #import "JCAutocompletingSearchViewControllerDelegate.h"
    
    @interface MySearchDelegate : NSObject <JCAutocompletingSearchViewControllerDelegate>
    @end
    
    #import "JCAutocompletingSearchViewController.h"
    
    @implementation MySearchDelegate
    
    - (void) searchController:(JCAutocompletingSearchViewController*)searchController
        performSearchForQuery:(NSString*)query
           withResultsHandler:(JCAutocompletingSearchResultsHandler)resultsHandler {
      resultsHandler(@[
        @{@"label": @"Result 1"},
        @{@"label": @"Result 2"},
        @{@"label": @"Result 3"}
      ]);
    }
    
    - (void) searchControllerCanceled:(JCAutocompletingSearchViewController*)searchController {
      // Handle search cancellation.
    }
    
    - (void) searchController:(JCAutocompletingSearchViewController*)searchController
                    tableView:(UITableView*)tableView
               selectedResult:(id)result {
      // Handle search result selection.
    }
    
    // Optional.
    - (BOOL) searchControllerShouldPerformBlankSearchOnLoad:(JCAutocompletingSearchViewController*)searchController {
      return YES;
    }
    
    @end
    
    // ------------------------------------------------------------------------
    
    - (BOOL) application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
      self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
      
      JCAutocompletingSearchViewController* searchController = [JCAutocompletingSearchViewController autocompletingSearchViewController];
      searchController.delegate = [MySearchDelegate new];
      
      UINavigationController* navigationController = [[UINavigationController alloc] initWithRootViewController:searchController];
      navigationController.navigationBarHidden = YES;
      self.window.rootViewController = navigationController;
      
      [self.window makeKeyAndVisible];
      return YES;
    }
    

Installation?
-------------

This project includes a `podspec` for usage with [CocoaPods](http://http://cocoapods.org/). Simply add

    pod 'JCAutocompletingSearch'

to your `Podfile` and run `pod install`.

Alternately, you can add all of the files contained in this project's `Library` directory to your Xcode project. If your project does not use ARC, you will need to enable ARC on these files.

Licence
-------

This project is licensed under the MIT license. All copyright rights are retained by myself.

Copyright (c) 2012 James Coleman

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.