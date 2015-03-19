---
layout: default
title: For Loops in Node.js
---

I was wondering when I’d hit this problem and I just did, looping through one DB’s artworks and posting it to another (doing this for testing the sync tools of Shazart's servers)

You can’t just do a for loop in javascript since everything is asynchronous, (read the article for more info)

Heres an article on doing a loop in node.js that waits for the callback of the previous call of the function to finish before it moves to the next
http://www.richardrodger.com/2011/04/21/node-js-how-to-write-a-for-loop-with-callbacks/#.VQo3tBCUcpA

Heres a snippet I want to keep around, I have a feels I'll be using it a lot.

	var filenames = [...]
	function uploader(i) {
	  if( i < filenames.length ) {
	    upload( filenames[i], function(err) {
	      if( err ) {
	        console.log('error: '+err)
	      }
	      else {
	        uploader(i+1)
	      }
	    })
	  }
	}
	uploader(0)

Sure this is just Comp Sci 201 but recursion is hard to get your head around sometimes, have some sample code stashed away when you get lost always helps.

Now I just need a template for a backtracking algorithm...
