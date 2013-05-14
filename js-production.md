## Requirejs archiving & 난독화 

***build.js***

	({
	    // baseurl
	    baseUrl: ".",
	    // library function argument name binding
	    shim: {
	        underscore: {
	            // require 시 function argument name을 명시할수있다.
	            exports: "_"
	        },
	        backbone: {
	            deps: ["underscore", "jquery"],
	            exports: "Backbone"
	        },
	        bootstrap: {
	            deps: ["jquery"],
	            exports: "b"
	        }
	    },
	    // 3rdparty library aliases
	    // 이렇게 하지않으면 require할때마다 졸라 길게 써야된다능 -_-a
	    paths: {
	        "underscore": "libs/underscore-min",
	        "backbone": "libs/backbone-min",
	        "bootstrap": "libs/bootstrap.min",
	        "jquery": "libs/jquery-1.9.0.min",
	        "text": "libs/text"
	    },
		name:"main",
		out:"main-built.js"
	})

***설정으로 build***

	  r.js -o build.js 
	Tracing dependencies for: main
	Uglifying file: /Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/main-built.js

	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/main-built.js
	----------------
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/libs/jquery-1.9.0.min.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/libs/bootstrap.min.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/libs/underscore-min.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/libs/backbone-min.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/libs/text.js
	text!templates/app.html
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/models/Nest.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/collections/NestList.js
	text!templates/list.html
	text!templates/item.html
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/views/NestItemView.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/views/NestListView.js
	text!templates/add.html
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/views/NestAddView.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/views/AppView.js
	/Users/bhkim/Documents/wenest-admin/src/main/webapp/resources/scripts/main.js

