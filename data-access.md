    

    
    
//      ========== CRUD data messaging workflow =========
// 
//      1 - UI => temporary update component data && send message to 'data.change.local' topic via Mediator 
//      
//      2 - Mediator PubSub Publisher send message to 'data.change.local' topic from PubSub
//          
//      2 - DAO PubSub Subscriber <= receive message from UI via PubSub and try to save on server
//          IF success
//              DAO save data locally
//              DAO => send succes message to 'data.change.remote' topic via Mediator
//          IF error
//              DAO => send error message to 'data.change.remote' topic via Mediator
//              
//      3 - UI PubSub Subscriber <= receive message 
//          IF success
//              UI update component data
//          IF error
//              UI display properly error message and delete temporary data

//      ========== CRUD data messaging  - message structure =========
//      message = {
//          action: 'create', // create, update, delete, list,
//          collection: 'users',
//          model: 'user',
//          data: {}
//          datetime: server_date_time, client_date_time
//          to: 'client' // client, server,
//          topic: 'data.change.remote' // 'data.change.remote', 'data.change.local'
//      }







    https://localforage.github.io/localForage/#settings-api-setdriver
    http://www.js-data.io/docs/dslocalforageadapter
    http://www.js-data.io/docs/home
    https://www.npmjs.com/package/js-data-localforage