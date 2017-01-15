---
layout: post
title:  "How to build the "nested" $http service"
date:   2017-01-15 10:38:01 -0500
---


In my final project, I built a todo list app which has the nested $http service. I stuck with writing codes for it, so I'd like to write about my sticking points.



![](http://i.imgur.com/nc1Tuu7.gif)



I had built the Dashboard page using $http service. $http service is the low level so I had to implement Read, Create,  and Delete actions by myself. 


*TodoListService.js*
```

'use strict';

// $q : A service that helps you run functions asynchronously,
// and use their return values (or exceptions) when they are done processing.

angular
  .module('todoApp')
  .factory('TodoListService', ['$http', '$q', function($http, $q){

    return {

      getAllTodoLists: function(){
        return $http.get('/api/todo_lists/')
                    .then(function(response){
                      return response.data;
                    },
                    function(errorResponse){
                      console.error('Error while getting lists');
                      return $q.reject(errorResponse);
                    });
      },

      findTodoList: function(id){
        return $http.get('/api/todo_lists/'+id)
                    .then(function(response){
                      return response.data;
                    },
                    function(errorResponse){
                      console.error('Error while finding list');
                      return $q.reject(errorResponse);
                    });
      },

      createTodoList: function(list){
        return $http.post('/api/todo_lists/', list)
                    .then(function(response){
                      return response.data;
                    },
                    function(errorResponse){
                      console.error('Error while creating list');
                      return $q.reject(errorResponse);
                    });
      },

      deleteTodoList: function(id){
        return $http.delete('/api/todo_lists/'+id)
                    .then(function(response){
                      return response.data;
                    },
                    function(errorResponse){
                      console.error('Error while deleting list');
                      return $q.reject(errorResponse);
                    });
      }
    };
  }]);
	
```

Then I built DashboardCtrl.js using TodoListService.js. 


*DashboardCtrl.js*

```

'use strict';

angular
  .module('todoApp')
  .controller('DashboardCtrl', ['$scope', 'TodoListService', function($scope, TodoListService){
    var self = this;
    self.list = { id: null, name: ''};
    self.lists = [];

    self.getAllTodoLists = function(){
      TodoListService.getAllTodoLists()
                     .then(function(data){
                       self.lists = data;
                     },
                     function(errorResponse){
                       console.error('Error while getting all lists');
                     });
    };

    self.lists = self.getAllTodoLists();

    self.createList = function(list){
      TodoListService.createTodoList(list)
                     .then(function(resp){
                       var list = resp;
                       self.lists.push({
                         id: list.id,
                         name: list.name,
                         todos: []
                       });
                       self.list.name = "";
                     }, function(errorResponse){
                       console.error('Error while creating list');
                     });
    };

    self.deleteList = function(id){
      TodoListService.deleteTodoList(id)
                     .then(function(resp) {
                       alert('Delete List?');
                       for(var i = 0; i < self.lists.length; i++ ){
                         if(self.lists[i].id === id)
                         self.lists.splice(i,1);
                       }
                     }, function(errorResponse){
                        console.error('Error while deleting list');
                     });
    };

  }]);
	
```

It was simple! 

Next, I tried to build Create, Update, and Delete functions which have "nested" $http requests in TodoService.js. But I stuck with it. How to implement the nested $http request in the codes? I searched a lot but I found only about the CRUD app with $resource service.

ummm...

After trial and error over and over again, I've finally built the $http service. I got hints from the deleteTodoList function in TodoListService.js, I added two or three arguments the $http service in TodoService.js.

*TodoService.js*

```

'use strict';

angular
   .module('todoApp')
   .factory('TodoService', ['$http', '$q', function($http, $q){

     return {

       createTodo: function(listId, todo){
         return $http.post('/api/todo_lists/'+ listId + '/todos/', todo)
                     .then(function(response){
                       return response.data;
                     },
                     function(errorResponse){
                       console.error('Error while creating todo');
                       return $q.reject(errorResponse);
                     });
       },

       updateTodo: function(listId, todo, id){
         return $http.put('/api/todo_lists/'+ listId + '/todos/'+id, todo)
                     .then(function(response){
                       return response.data;
                     },
                     function(errorResponse){
                       console.error('Error while updating todo');
                       return $q.reject(errorResponse);
                     });
       },

       deleteTodo: function(listId, id){
         return $http.delete('/api/todo_lists/'+listId + '/todos/'+id)
                     .then(function(response){
                       return response.data;
                     },
                     function(errorResponse){
                       console.error('Error while deleting todo');
                       return $q.reject(errorResponse);
                     });
       }
     };
}]);

```

I stuck with how to interpret the parent id, so I solved the problem to set that as an argument. For example, The createTodoList function in TodoListService.js has taken an argument, the list data in that function, on the other hand, The createTodo function in TodoService.js has taken two arguments, the todo data and the parent id in that function. 


*The createTodoList function in TodoListService.js*

```

angular
  .module('todoApp')
  .factory('TodoListService', ['$http', '$q', function($http, $q){

    return {
		
		.
		.
		.
		
		createTodoList: function(list){
        return $http.post('/api/todo_lists/', list)
                    .then(function(response){
                      return response.data;
                    },
                    function(errorResponse){
                      console.error('Error while creating list');
                      return $q.reject(errorResponse);
                    });
      },
			
	  .
		.
		.

   };
  }]);
```


*The createTodo function in TodoService.js*

```

'use strict';

angular
   .module('todoApp')
   .factory('TodoService', ['$http', '$q', function($http, $q){

     return {
		 
		 createTodo: function(listId, todo){
         return $http.post('/api/todo_lists/'+ listId + '/todos/', todo)
                     .then(function(response){
                       return response.data;
                     },
                     function(errorResponse){
                       console.error('Error while creating todo');
                       return $q.reject(errorResponse);
                     });
       },
			 
			 .
			 .
			 .
		 
     };
}]);

```

Then I built TodoListCtrl.js using TodoService.js. 

```

'use strict';

angular
  .module('todoApp')
  .controller('TodoListCtrl', ['$scope', '$stateParams', 'TodoListService', 'TodoService', function($scope, $stateParams, TodoListService, TodoService){

    var self = this;
    var myListId = $stateParams.list_id;
    self.todo = { id: null, todo_list_id: myListId, description: '', done: false };

    self.getAllTodos = function(myListId){
      TodoListService.findTodoList(myListId)
                     .then(function(data){
                       self.list = data;
                     },
                     function(errorResponse){
                       console.error('Error while gatting todos');
                     });
    };

    self.createTodo = function(myListId, todo){
      TodoService.createTodo(myListId, todo)
                 .then(self.getAllTodos(myListId), function(errorResponse){
                   console.error('Error while creating todo');
                 });
    };

    self.updateTodo = function(myListId, todo, id){
      TodoService.updateTodo(myListId, todo, id)
                 .then(self.getAllTodos(myListId), function(errorResponse){
                   console.error('Error while updating todo');
                 });
    }

    self.deleteTodo = function(myListId, id){
      TodoService.deleteTodo(myListId, id)
                 .then(self.getAllTodos(myListId), function(errorResponse){
                   console.error('Error while deleting todo');
                 });
    }

    self.list = self.getAllTodos(myListId);

    self.addTodo = function(){
      self.createTodo(myListId, self.todo);
      self.reset();
    };

    self.reset = function(){
      self.todo = { id: null, todo_list_id: myListId, description: '', done: false };
    };

    self.removeTodo = function(id){
      self.deleteTodo(myListId, id);
    };

    self.toggleTodo = function(todo, id){
      self.updateTodo(myListId, self.todo.done = {
        done: todo.done
      }, id);
    };

    self.filter = {
      done: { done: true },
      remaining: { done: false }
    };

    self.changeFilter = function(filter){
      self.currentFilter = filter;
    };

  }]);
	
```

I've done!

![](http://i.imgur.com/krtE5RZ.gif)

I learned when I want to build the nested $http service, I can set the parent id as an argument to the child function. I will try to create the deeper nested service in the future!!






