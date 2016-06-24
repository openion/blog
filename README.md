----

> 创建脚本并自动检索错误基本位置

    #!/bin/bash
    #作者：openion
    #说明：创建脚本并自动检索错误基本位置
    #时间：12/24/15
    
    if ! grep "[^[:space:]]" $1 &> /dev/null;then
        echo "#!/bin/bash
    	# Name: ${name}.sh
    	# Description: $des
    	# Author: $author
    	# Version: 0.0.1
    	# Datatime: `date "+%F %T"`
    	# usage: ...     
    	# Other：...
    
    	" > $1
    	chmod +x $1
    	vim +10 $1
    else
    	vim + $1
    fi
    
    until  bash -n $1 &> /dev/null; do
            read -p "Syntax error,[q|Q] for quiting,others for editing: " OPT
            case $OPT in
              q|Q)
                    echo "Quit..."
                    exit 1
    		rm -rf file &> /dev/null;;
              *)
    		tmp=`bash -n $1 &>file; cat file | sed -n '1p' | cut -d: -f2 | awk '{print $2}'`
                    vim +$tmp $1;;
            esac
    done
    rm -rf file &> /dev/null
    ./$1 &>file
    until `grep "[0-9]" file`; do
            read -p "Function error,[q|Q] for quiting,others for editing: " OPT
            case $OPT in
              q|Q)
                    echo "Quit..."
                    exit 1
    		rm -rf file &> /dev/null;;
              *)
                    tmp=`cat file | grep "line [0-9]" | sed -n '1p' | cut -d: -f2 | awk '{print $2}'`
                    vim +$tmp $1
            	./$1 &>file;;
    	esac
    done
    rm -rf file &> /dev/null
