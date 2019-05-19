---
title: "本地数据存储之 IndexedDB 实践"
date: 2014-12-14T10:57:34+08:00
draft: false
tags: []
keywords: []
description: ""
slug: ""
---

## 1. 本地存储的类型

本地存储主要有以下几种：

- Web Sql
- IndexedDB
- Local Storage
- Session Storage
- Cookies
- Application Cache

## 2. 项目需求

- 离线存储读取数据
- 允许用户对数据进行增删改操作
- 数据存储在本地，不依赖后端
- 数据支持索引查询

由于 WebSQL 在标准上还存在争议，而 localStorage 实现数据分页、查询比较复杂，最终考虑使用了 IndexedDB 来满足需求。

## 3. IndexedDB 　数据库的使用

### 3.1 定义数据库对象

    var wxbDB = {
        name   : "wxb",
        version: 1,
        db     : null
    };

### 3.2 数据库初始化

    function initDB(dbObj) {
        dbObj.version = dbObj.version || 1;
        var request = indexedDB.open(dbObj.name, dbObj.version);
        request.onerror = function (e) {
            console.log(e.currentTarget.error.message);
        };
        request.onsuccess = function (e) {
            dbObj.db = e.target.result;
        };
        request.onupgradeneeded = function (e) {
            var thisDB = e.target.result;
            if (!thisDB.objectStoreNames.contains("material")) {
                var objStore = thisDB.createObjectStore("material", {keyPath: "id", autoIncrement: true});
                objStore.createIndex("wxid", "wxid", {unique: true});
            }
            if (!thisDB.objectStoreNames.contains("account")) {
                var objStore = thisDB.createObjectStore("account", {keyPath: "id", autoIncrement: true});
                objStore.createIndex("wxid", "wxid", {unique: true});
                objStore.createIndex("nickName", "nickName", {unique: false});
            }
        };
    }

### 3.3 关闭数据库

    function closeDB(dbObj) {
        dbObj.db.close();
    }

### 3.4 删除数据库

    function deleteDB(dbObj) {
        indexedDB.deleteDatabase(dbObj.name);
    }

### 3.5 数据库表的 CURD

#### 3.5.1 添加数据

    	function addData(dbObj, tableName, data, cb) {
    	    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    	    transaction.oncomplete = function () {
    	        console.log("transaction complete");
    	    };
    	    transaction.onerror = function (event) {
    	        console.dir(event)
    	    };

    	    var objectStore = transaction.objectStore(tableName);
    	    var request = objectStore.add(data);
    	    request.onsuccess = function (e) {
    	        if (cb) {
    	            cb({
    	                error: 0,
    	                data : data
    	            })
    	        }
    	    };
    	    request.onerror = function (e) {
    	        if (cb) {
    	            cb({
    	                error: 1
    	            })
    	        }
    	    }
    	}

#### 3.5.2 删除数据

    	function deleteData(dbObj, tableName, id, cb) {
    	    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    	    transaction.oncomplete = function () {
    	        console.log("transaction complete");
    	    };
    	    transaction.onerror = function (event) {
    	        console.dir(event)
    	    };
    	    var objectStore = transaction.objectStore(tableName);
    	    var request = objectStore.delete(parseInt(id));
    	    request.onsuccess = function (e) {
    	        if (cb) {
    	            cb({
    	                error: 0,
    	                data : parseInt(id)
    	            })
    	        }
    	    };
    	    request.onerror = function (e) {
    	        if (cb) {
    	            cb({
    	                error: 1
    	            })
    	        }
    	    }
    	}

#### 3.5.3 查询数据

##### 3.5.3.1 获取全部数据

    		function getDataAll(dbObj, tableName, cb) {
    		    var transaction = dbObj.db.transaction(tableName, 'readonly');
    		    transaction.oncomplete = function () {
    		        console.log("transaction complete");
    		    };
    		    transaction.onerror = function (event) {
    		        console.dir(event)
    		    };

    		    var objectStore = transaction.objectStore(tableName);
    		    var rowData = [];
    		    objectStore.openCursor(IDBKeyRange.lowerBound(0)).onsuccess = function (event) {
    		        var cursor = event.target.result;
    		        if (!cursor && cb) {
    		            cb({
    		                error: 0,
    		                data : rowData
    		            });
    		            return;
    		        }
    		        rowData.push(cursor.value);
    		        cursor.continue();
    		    };
    		}

#### 3.5.3.2 根据 id 获取数据

    		function getDataById(dbObj, tableName, id, cb) {
    		    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    		    transaction.oncomplete = function () {
    		        console.log("transaction complete");
    		    };
    		    transaction.onerror = function (event) {
    		        console.dir(event)
    		    };

    		    var objectStore = transaction.objectStore(tableName);
    		    var request = objectStore.get(id);
    		    request.onsuccess = function (e) {
    		        if (cb) {
    		            cb({
    		                error: 0,
    		                data : e.target.result
    		            })
    		        }
    		    };
    		    request.onerror = function (e) {
    		        if (cb) {
    		            cb({
    		                error: 1
    		            })
    		        }
    		    }
    		}

#### 3.5.3.3 根据关键词索引获取数据

    		function getDataBySearch(dbObj, tableName, keywords, cb) {
    		    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    		    transaction.oncomplete = function () {
    		        console.log("transaction complete");
    		    };
    		    transaction.onerror = function (event) {
    		        console.dir(event)
    		    };

    		    var objectStore = transaction.objectStore(tableName);
    		    var boundKeyRange = IDBKeyRange.only(keywords);
    		    var rowData;
    		    objectStore.index("nickName").openCursor(boundKeyRange).onsuccess = function (event) {
    		        var cursor = event.target.result;
    		        if (!cursor) {
    		            if (cb) {
    		                cb({
    		                    error: 0,
    		                    data : rowData
    		                })
    		            }
    		            return;
    		        }
    		        rowData = cursor.value;
    		        cursor.continue();
    		    };
    		}

#### 3.5.3.4 根据页码获取数据

    		function getDataByPager(dbObj, tableName, start, end, cb) {
    		    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    		    transaction.oncomplete = function () {
    		        console.log("transaction complete");
    		    };
    		    transaction.onerror = function (event) {
    		        console.dir(event)
    		    };

    		    var objectStore = transaction.objectStore(tableName);
    		    var boundKeyRange = IDBKeyRange.bound(start, end, false, true);
    		    var rowData = [];
    		    objectStore.openCursor(boundKeyRange).onsuccess = function (event) {
    		        var cursor = event.target.result;
    		        if (!cursor && cb) {
    		            cb({
    		                error: 0,
    		                data : rowData
    		            });
    		            return;
    		        }
    		        rowData.push(cursor.value);
    		        cursor.continue();
    		    };
    		}

### 3.5.4 更新数据

    	function updateData(dbObj, tableName, id, updateData, cb) {
    	    var transaction = dbObj.db.transaction(tableName, 'readwrite');
    	    transaction.oncomplete = function () {
    	        console.log("transaction complete");
    	    };
    	    transaction.onerror = function (event) {
    	        console.dir(event)
    	    };

    	    var objectStore = transaction.objectStore(tableName);
    	    var request = objectStore.get(id);
    	    request.onsuccess = function (e) {
    	        var thisDB = e.target.result;
    	        for (key in updateData) {
    	            thisDB[key] = updateData[key];
    	        }
    	        objectStore.put(thisDB);
    	        if (cb) {
    	            cb({
    	                error: 0,
    	                data : thisDB
    	            })
    	        }
    	    };
    	    request.onerror = function (e) {
    	        if (cb) {
    	            cb({
    	                error: 1
    	            })
    	        }
    	    }
    	}

ps: 实现细节部分待有时间进行补充完善～

参考文章：http://www.smashingmagazine.com/2014/09/02/building-simple-cross-browser-offline-todo-list-indexeddb-websql/
