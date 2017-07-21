#!/bin/bash

copyright="/*
 * Copyright (c) $(date +%Y) by Marfeel Solutions (http://www.marfeel.com)
 * All Rights Reserved.
 *
 * NOTICE:  All information contained herein is, and remains
 * the property of Marfeel Solutions S.L and its suppliers,
 * if any.  The intellectual and technical concepts contained
 * herein are proprietary to Marfeel Solutions S.L and its
 * suppliers and are protected by trade secret or copyright law.
 * Dissemination of this information or reproduction of this material
 * is strictly forbidden unless prior written permission is obtained
 * from Marfeel Solutions SL.
 */

 "

function errorMsg(){
  echo "- Incorrect file name :" "$fileName"
  echo "- Please use only letters. First letter must be CamelCase!"
  echo "- Example of a correct file name: MyCustomWidget"
  echo "- Please fix it and try again."
}

  function createClass(){
    $(sed -i '' '15i\
      import m from "Marfeel";\
      \
        export default class '$1' {\
          \  constructor()\
        }\
        \
        m.definePlugin("'$3'", function () {\
        \  return new '$1'(this);\
        });

        ' $2)
 }

  function registerWidget(){
  $(sed -i '' '/export.*default.*{.*/i\
        CustomWidgets.registerWidget({selector: "", name: "'$1'"});
        ' main.js)
 }

 function importWidget() {
  $(sed -i '' '/metrics\/Metrics.*/a\
  import CustomWidgets from "marfeel/touch/widgets/CustomWidgets";
        ' main.js)
 }

if [ "$1" ] ; then
fileName=$1
if [[ $1 =~ ^[A-Z][a-zA-Z]*$ ]] ; then
  file=$fileName".js"
  if [ -e "$file" ]; then
    echo "File exists! Aborting..."
    exit 1
  else
    touch $file
  fi
    if [ "$file" ] ; then
      widgetName=$(echo ${file%.js} | tr '[:upper:]' '[:lower:]')
      isWidgetRegistered=$(grep "$widgetName" main.js | wc -l)
        if [ "$isWidgetRegistered" -gt 0 ] ; then
          echo "- Widget name is already registered or exists ($widgetName). Aborting..."
          exit 1
        fi
      echo "- Creating file:" "$file"
      echo "$copyright" > $file
      createClass $fileName $file $widgetName
      echo "- Widget name:" $widgetName
      import=$(grep "widgets/CustomWidgets" main.js | wc -l)
      if [ "$import" -eq 1 ] ; then
        echo "- Import found."
        echo "- Registering widget with name:" $widgetName
        registerWidget $widgetName
      else
        echo "- Missing import! Importing CustomWidgets..."
        importWidget
        echo "- Registering widget with name:" $widgetName
        registerWidget $widgetName
      fi
    fi
  else
    errorMsg $fileName
  fi
fi
