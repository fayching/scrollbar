
// 构造函数
var ScrollBar = function(el,option){
    var defaults = {
        step: 1,
        flag: false,
        startIndex: 0,
        startY: 0,
        callback: function(){}
    }
    this.element = $(el);
    this.option = $.extend(defaults,option);
    this.trigger = this.option.trigger;
    this.scrolling = this.option.scrolling;
    this.before = this.option.before;
    this.start = this.option.start;
    this.end = this.option.end;
    this._init();
    this._bindTrigger();
}
ScrollBar.prototype = {
    _init : function(){
        this.before();
        $(this.trigger).show(); 
        this.option.top = Math.ceil($(this.element).offset().top);
        this.option.height = this.element.height();
        this.option.length = this.element.children().length;
        this.option.itemHeight = this.element.children().height();
        this.option.triggerOffsetY = $(this.trigger).height() * this.option.step/this.option.length; //滑动条单位长度
        this.lastY = $(this.trigger).offset().top - document.body.scrollTop;
        
    },
    refresh: function(){
        this.option.height = this.element.height();
        this.option.length = this.element.children().length;
        this.option.height = this.option.itemHeight * this.option.length;
        this.option.triggerOffsetY = $(this.trigger).height() * this.option.step/this.option.length;
    },
    _move : function(e){
        var triggerTop = e.changedTouches[0].pageY - $(this.trigger).offset().top;
        var addIndex = Math.floor(triggerTop/this.option.triggerOffsetY) * this.option.step;
        var startIndex = this.option.startIndex;
        var currentIndex = startIndex + addIndex;
        if(currentIndex <  startIndex || currentIndex > startIndex + this.option.length ) return;
        document.body.scrollTop = this.option.top - this.option.startY;
        this.element.css({'transform' : 'translate(0, '+ (-addIndex * this.option.itemHeight) +'px) translateZ(0px)'});
        this.lastY = e.changedTouches[0].clientY;
        var endIndex = currentIndex + this.option.step;
        if(currentIndex >= startIndex + this.option.length - this.option.step) {
            endIndex = startIndex + this.option.length;
        }
        
        this.currentIndex = currentIndex;
        this.scrolling(this.currentIndex,endIndex,triggerTop);
    },
    _bindTrigger : function(){
        var _self = this;
        $(this.trigger).on('touchstart', function(e){
            e.preventDefault();
            _self.start();
            $(_self.trigger).show();
            _self._move(e);
            _self.option.flag = true;
        });
        $(this.trigger).on('touchmove',function(e){
            if(!_self.option.flag) return;
            e.preventDefault();
            if(Math.abs(e.changedTouches[0].clientY - _self.lastY) > _self.option.triggerOffsetY){
                _self._move(e);
            } 
        });
        $(this.trigger).on('touchend',function(e){
            document.body.scrollTop =(_self.currentIndex - _self.option.startIndex) * _self.option.itemHeight + _self.option.top - _self.option.startY;
            _self.element.css({'transform' : 'translate(0, 0) translateZ(0px)'});
            _self.option.flag = false;
            _self.end();
        });
    }

}
