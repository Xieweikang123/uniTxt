<template>
	<view>
		2333
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
				var CallLog = plus.android.importClass('android.provider.CallLog');
			 	var main = plus.android.runtimeMainActivity();
			 	var obj = main.getContentResolver();
				plus.android.importClass(obj);
				console.log("CallLog.Calls.CONTENT_URI",CallLog.Calls.CONTENT_URI	);
					
				// var map=[];
				// map[CallLog.Calls.NUMBER]=15555555555;
				// map[CallLog.Calls.DURATION]=30;
				// values.push({
				// 	CallLog.Calls.NUMBER:155555555555,
				// 	// CallLog.Calls.DATE:new Date().getTime(),
				// 	CallLog.Calls.DURATION:30
				// })
				console.log("时间戳",Date.parse(new Date()))
				var contentValues = plus.android.newObject("android.content.ContentValues");
				 plus.android.invoke(contentValues,"put",CallLog.Calls.CACHED_NAME,"name");
				 plus.android.invoke(contentValues,"put",CallLog.Calls.NUMBER,"15555555555");
				 plus.android.invoke(contentValues,"put",CallLog.Calls.DATE,Date.parse(new Date()));
				 plus.android.invoke(contentValues,"put",CallLog.Calls.DURATION,30);
				 plus.android.invoke(contentValues,"put",CallLog.Calls.TYPE,1);
				 // plus.android.invoke(contentValues,"put",CallLog.Calls.INCOMING_TYPE ,0);
				 plus.android.invoke(contentValues,"put",CallLog.Calls.NEW,0);
				 
				console.log("contentValues",plus.android.invoke(contentValues,"get",CallLog.Calls.NUMBER))
				var inserCursor=obj.insert(CallLog.Calls.CONTENT_URI, contentValues);
				console.log("inserCursor",inserCursor);
				var cursor = obj.query(CallLog.Calls.CONTENT_URI,null,null,null,null);
					
							
			 	plus.android.importClass(cursor);
				
			 	// var String = plus.android.importClass("java.lang.String");
			 	var content = [];
				var count=0;
				
				if(cursor.moveToFirst()){
					while(cursor.moveToNext()){
						count++;
						//号码
						var number=cursor.getString(cursor.getColumnIndex(CallLog.Calls.NUMBER));
						//时间
						var date=new Date(parseInt(cursor.getString(cursor.getColumnIndexOrThrow(CallLog.Calls.DATE))));
						//联系人
						var contactName=cursor.getString(cursor.getColumnIndexOrThrow(CallLog.Calls.CACHED_NAME));
						
						// console.log("number",number,date);
						content.push({
							contactName:contactName,
							number:number,
							date:date
						});
					}
				}
				console.log("content",content );
			
			 	// while (plus.android.invoke(cs, "moveToNext")) {
			 	// 	talist.push({
			 	// 		name: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.CACHED_NAME)),
			 	// 		mobile: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.NUMBER)),
			 	// 		talkTime: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.DURATION)),
			 	// 		callTime: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.DATE)),
			 	// 		numberLocation: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.GEOCODED_LOCATION)),
			 	// 		type: plus.android.invoke(cs, "getString", plus.android.invoke(cs, "getColumnIndex", CallLog.Calls.TYPE)) == 2 ?
			 	// 			"未接" : "已接"
			 	// 	});
			 	// }
			// console.log("callLog",CallLog);
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
