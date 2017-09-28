var pageTest = {}

pageTest.openUrl = function (page,action,callback) {
    var args = action.args
    page.open(args.url, function (status) {
        if(status == 'success'){

            setTimeout(function () {
                page.evaluate(action.evaluateFunction,args);
                if(args.cap){
                    page.render(args.capname)
                }
                callback();
            },args.timeout)

        }
    })
}
pageTest.changeUrl = function (page,action, callback) {
    var args = action.args

    setTimeout(function(){
        page.evaluate(action.evaluateFunction,args)
        if(args.cap){
            page.render( args.capname)
        }
        callback()
    },args.timeout)

    // page.open(url, function (status) {
    //     if(status == 'success'){
    //         if(cap){
    //             page.render('warning.png')
    //         }
    //         setTimeout(function () {
    //             console.log('-------inner-----------')
    //             callback()
    //         },timeout)
    //     }
    // })
}

module.exports= pageTest