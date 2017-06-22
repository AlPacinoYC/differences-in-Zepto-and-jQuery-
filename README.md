# differences-in-Zepto-and-jQuery-
使用document.createDocumentFragment()创建的dom用$("dom").html()添加上去，
jquery可以将这个dom加到jQ数组对象的所有元素上去，
zepto只能加到数组对象的第一个元素上去。

看了一下源码
zepto:
  html: function(html) {
      return html === undefined ?
      //参数html不存在时，获取集合中第一条记录的html
      (this.length > 0 ? this[0].innerHTML : null) :
      //否则遍历集合，设置每条记录的html
      this.each(function(idx) {
        //记录原始的innerHTMl
        var originHtml = this.innerHTML
        //如果参数html是字符串直接插入到记录中，
        //如果是函数，则将当前记录作为上下文，调用该函数，且传入该记录的索引和原始innerHTML作为参数
        $(this).empty().append(funcArg(this, html, idx, originHtml))
      })
    },
   jQuery:
   
	html: function( value ) {
		return access( this, function( value ) {
			var elem = this[ 0 ] || {},
				i = 0,
				l = this.length;

			if ( value === undefined && elem.nodeType === 1 ) {
				return elem.innerHTML;
			}

			// See if we can take a shortcut and just use innerHTML
			if ( typeof value === "string" && !rnoInnerhtml.test( value ) &&
				!wrapMap[ ( rtagName.exec( value ) || [ "", "" ] )[ 1 ].toLowerCase() ] ) {

				value = jQuery.htmlPrefilter( value );

				try {
					for ( ; i < l; i++ ) {
						elem = this[ i ] || {};

						// Remove element nodes and prevent memory leaks
						if ( elem.nodeType === 1 ) {
							jQuery.cleanData( getAll( elem, false ) );
							elem.innerHTML = value;
						}
					}

					elem = 0;

				// If using innerHTML throws an exception, use the fallback method
				} catch ( e ) {}
			}

			if ( elem ) {
				this.empty().append( value );
			}
		}, null, value, arguments.length );
	},
  
  
  jQuery缓存了value再append，而zepto只是append初始value
