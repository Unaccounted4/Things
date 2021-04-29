#!/bin/sh

# Function to find the real directory a program resides in.
# Feb. 17, 2000 - Sam Lantinga, Loki Entertainment Software
FindPath()
{
    fullpath="`echo $1 | grep /`"
    if [ "$fullpath" = "" ]; then
        oIFS="$IFS"
        IFS=:
        for path in $PATH
        do if [ -x "$path/$1" ]; then
               if [ "$path" = "" ]; then
                   path="."
               fi
               fullpath="$path/$1"
               break
           fi
        done
        IFS="$oIFS"
    fi
    if [ "$fullpath" = "" ]; then
        fullpath="$1"
    fi

    # Is the sed/ls magic portable?
    if [ -L "$fullpath" ]; then
        #fullpath="`ls -l "$fullpath" | awk '{print $11}'`"
        fullpath=`ls -l "$fullpath" |sed -e 's/.* -> //' |sed -e 's/\*//'`
    fi
    dirname $fullpath
}

if [ "${JAMESTOWN_DATA_PATH}" = "" ]; then
    JAMESTOWN_DATA_PATH="`FindPath $0`"
fi

echo "Jamestown: Installed in '$JAMESTOWN_DATA_PATH'."
cd "$JAMESTOWN_DATA_PATH"

# Update the install path symlink, in case the install moved.
mkdir -p "$HOME/.jamestown"
rm -f "$HOME/.jamestown/install_path"
ln -s "`pwd`" "$HOME/.jamestown/install_path"

if [ "${JAMESTOWN_ARCH}" = "" ]; then
    JAMESTOWN_ARCH=`uname -m`
fi

if [ "${JAMESTOWN_ARCH}" = "x86_64" ]; then
    echo "Jamestown: Using amd64 version."
    LD_LIBRARY_PATH=./x86:$LD_LIBRARY_PATH exec ./Jamestown-x86 "$@"
else
    echo "Jamestown: Using x86 version."
    LD_LIBRARY_PATH=./x86:$LD_LIBRARY_PATH exec ./Jamestown-x86 "$@"
fi

