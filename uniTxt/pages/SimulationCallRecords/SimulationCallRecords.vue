<template>
	<view>
		<view style="padding: 21px 56px;">输入电话号码 空格分隔，如(11111111111 111111111112)</view>
		<view class="flexJustifyContentCenter">

			<textarea maxlength="-1" @blur="clearUpTextarea()" class="grayBorder" v-model="phoneArrayValue"></textarea>

		</view>
		<view style="margin-top: 10px;" class="flexJustifyContentCenter">
			开始时间：日期
			<picker class="grayBorder" mode="date" :value="startDate" @change="bindStartDateChange">
				<view class="uni-input">{{startDate}}</view>
			</picker>时间
			<picker class="grayBorder" mode="time" :value="startTime" @change="bindStartTimeChange">
				<view class="uni-input">{{startTime}}</view>
			</picker>
		<!-- 	结束时间：
			<picker class="grayBorder" mode="time" :value="endTime" @change="bindEndTimeChange">
				<view class="uni-input">{{endTime}}</view>
			</picker> -->
		</view>
		<button @click="createCallLog()" :loading="createLogLoading" style="margin-top: 26px;width: 74%;" type="primary">生成通话记录</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				startDate:'1997-10-13',
				startTime: '10:00',
				endTime: '21:00',
				createLogLoading: false,
				phoneArrayValue: "13783503335 17838383339 15981919995 15936235558 18838978885 18860356888 18737100016 18203613579 15038383558 13598870870 18803712125 15903712199 15703711151 13903714123 15138695512 15290895512 18300683006 15890658906 15238383807 15238383878 15093199959 15136299919 15093288838 15093177797 15290877767 15713866676 15713866616 18237111171 15937100095 15038200010 15937100010 15093135522 15093219090 15093218383 15093128181 15093217676 15039077676 15093216363 15093226060 15225065959 15838115550 15890035557 18236760003 15838210003 15838210002 15937140001 13598040001 13598840001 15890198588 15290897788 15936257788 15238331100 15038330099 15138663966 15136259369 15238693369 14788868168 18737103168 18737130168 15737131168 15237108168 18703835168 15138970168 18336010123 18703670367 13721433555 15238653111 15238305000 18236436789 18203646789 15249686789 13526751234 17803995999 15290891999 13783591999 13733881999 15238066999 19838883888 19837111888 15937186888 13938251777 13838575777 18239961666 15238382666 13526610666 13703923456 15136454444 15138464444 15093336666 13526677777 13673997733 15803713368 15803712168 13703825151 13783685050 13633852525 13526861212 13592675050 13526623838 13673677171 13592677070 13838125522 13939015511 13676936299 15981861799 13838297099 13783632388 15981963677 13523012877 15803837866 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 15038163789 17837153789 15736713789 18738169567 17836902567 13598013678 13783509678 13783569567 13592616567 13603718907 "
			}
		},
		onLoad() {
			var nowDate=new Date();
			console.log("nowdate",nowDate)
			this.startDate=nowDate.getFullYear()+"-"+(nowDate.getMonth()+1)+"-"+nowDate.getUTCDate();
			console.log("startDate",this.startDate)
		},
		methods: {
			//textarea 整理	
			clearUpTextarea(){
				this.phoneArrayValue=this.phoneArrayValue.replace(/[\ +\r\n]/g,""); 
				var newStr=""
				//每隔11位 加空格
				for(var i=0;i<this.phoneArrayValue.length;i++){
					newStr+=this.phoneArrayValue[i]
					if((i+1)%11==0){
						newStr+=" ";
					}
				}
				this.phoneArrayValue=newStr;
				// console.log("this.newStr",newStr)
			},
			bindStartDateChange(e){
				console.log("bindStartDateChange",e)
				this.startDate=e.detail.value;
			},
			bindEndTimeChange(e) {
				if (this.startTime > e.target.value) {
					uni.showToast({
						title: '开始时间不能大于结束时间',
						duration: 3000,
						icon: "none"
					});
					return;
				}
				this.endTime = e.target.value;
			},
			bindStartTimeChange(e) {
				if (this.endTime < e.target.value) {
					uni.showToast({
						title: '开始时间不能大于结束时间',
						duration: 3000,
						icon: "none"
					});
					return;
				}
				this.startTime = e.target.value;
				console.log(this.startTime)
			},
			getTimeDifferenceSecond(startTime, endTime) {
				var diffHour = endTime.split(":")[0] - startTime.split(":")[0];
				var diffMinute = endTime.split(":")[1] - startTime.split(":")[1];
				console.log("diffHour", diffHour, diffMinute)
				return diffHour * 60 * 60 + diffMinute * 60;
			},
			//通话时长 //通话时长0，100个电话出现几率是20%  60秒到180秒几率是60%  3分钟到8分钟为15%几率  10分钟到20分钟5%
			randomCallTime(){
				var randomSeed = Math.floor((Math.random() * 100) + 1);
				if(randomSeed<=20){
					return 0;
				}else if(randomSeed<=85){
					return Math.floor((Math.random() * 121) + 60);
				}else if(randomSeed<=95){
					return Math.floor((Math.random() * 301) + 180);
				}else{
					return Math.floor((Math.random() * 601) + 600);
				}
				// console.log(randomSeed)
			},
			//生成
			//这个合理的解释就是  第二个电话最起码是第一个电话通话结束时间的顺延30秒以上
			createCallLog() {
				
				uni.showLoading({
					title: '生成中'
				});
				var phoneArray = this.phoneArrayValue.split(' ').filter(s => s && s.trim());

				
				//时间差 开始日期和结束日期之间相差的秒数
				var diffSecond = this.getTimeDifferenceSecond(this.startTime, this.endTime);
				console.log("difftime", diffSecond)
				//每隔多久打一次电话
				var callInterval=diffSecond/phoneArray.length;
				console.log("callInterval",callInterval)
				console.log("phonearray",phoneArray);
				
				//通话开始时间
				var callStartDate = new Date();
				callStartDate.setFullYear(this.startDate.split('-')[0])
				callStartDate.setMonth(this.startDate.split('-')[1])
				callStartDate.setUTCDate(this.startDate.split('-')[2])
				callStartDate.setHours(this.startTime.split(":")[0]);
				callStartDate.setMinutes(this.startTime.split(":")[1]);
				callStartDate.setSeconds(0);
				
				// console.log("lastCallEndTime",lastCallEndTime)
				for (var i = 0; i < phoneArray.length; i++) {
					// console.log("callStartDate",callStartDate)
					var dateHour = callStartDate.getHours();
					var dateMinute = callStartDate.getMinutes();
					var dateSecond = callStartDate.getSeconds();
					
					var afterCallDate=new Date();
					afterCallDate.setFullYear(this.startDate.split('-')[0])
					afterCallDate.setMonth(this.startDate.split('-')[1])
					afterCallDate.setUTCDate(this.startDate.split('-')[2])
					afterCallDate.setHours(callStartDate.getHours())
					afterCallDate.setMinutes(callStartDate.getMinutes())
					afterCallDate.setSeconds(callStartDate.getSeconds())
					// var afterObj={hours:callStartDate.getHours(),minutes:callStartDate.getMinutes(),seconds:callStartDate.getSeconds()};
					//第一次通话开始时间为开始时间
					//然后随机一个通话时间
					//记录上次通话结束时间=上次结束时间+随机通话时间
					
					//接下来通话开始时间：上次通话时间+顺延30秒
					
					//一个随机时间
					// var randomSecond = Math.floor((Math.random() * 60) + 1);
					//通话时长
					var callSpanTime=this.randomCallTime()
					this.addSecondToTime(afterCallDate,callSpanTime)
					//秒数够60s,分钟进1
					// if (dateSecond + callSpanTime >= 60) {
					// 	//几分钟
					// 	var remaindMinute=Math.floor((dateSecond+callSpanTime)/60);
					// 	// console.log("remainMinute",remaindMinute)
					// 	//分钟进1够60分钟，小时进1 ;分钟设置为0 
					// 	if (dateMinute + remaindMinute >= 60) {
					// 		//多几个小时
					// 		var remaindHour=Math.floor((dateMinute+remaindMinute)/60);
					// 		// afterCallDate.setHours(dateHour + remaindHour);
					// 		afterObj.hours=dateHour + remaindHour;
					// 		afterCallDate.setMinutes((dateMinute + remaindMinute )%60);
							
					// 	} else { //分钟加1不够60
					// 		afterCallDate.setMinutes(dateMinute + remaindMinute);
					// 	}
					// 	afterCallDate.setSeconds((dateSecond + callSpanTime)%60);
					// } else {
					// 	afterCallDate.setSeconds(dateSecond + callSpanTime);
					// }
					console.log(phoneArray[i], callSpanTime, callStartDate)
					// console.log("afterCallDate:",afterCallDate)
					
					callStartDate=afterCallDate
				
					//通话结束后 顺延30~60秒
					this.addSecondToTime(callStartDate,Math.floor((Math.random() * 30) + 30))
					// console.log("after shunyan",callStartDate)
					
					// this.insertCallLog(phoneArray[i], callStartDate, callSpanTime, 2, 0);
				}
				uni.showToast({
					title: '生成成功',
					duration: 3000
				});
				// this.createLogLoading=false;

				// console.log("phonearray", phoneArray)
			},
			addSecondToTime(date,second){
				var curSecond=date.getSeconds();
				var totalSeconds=curSecond+second;
				if(totalSeconds>=60){
					var remainMinutes=Math.floor(totalSeconds/60);
					var totalMinutes=date.getMinutes()+remainMinutes;
					//超过60分钟
					if(totalMinutes>=60){
						var remainHours=Math.floor(totalMinutes/60);
						date.setHours(date.getHours()+remainHours);}
						// date.setMinutes(totalMinutes%60)
					// }else{
					// 	date.setMinutes(totalMinutes)
					// }
					date.setMinutes(totalMinutes%60)
					date.setSeconds(totalSeconds%60);
				}else{
					date.setSeconds(totalSeconds)
				}
				return date;
			},
			// #ifdef APP-PLUS
			//插入一条通话记录 callDuration：通话时长(秒) callType: 1呼入 2呼出 3未接 ;  callNew 是否已查看    0已看1未看
			insertCallLog(number, callDate, callDuration, callType, callNew) {
				var CallLog = plus.android.importClass('android.provider.CallLog');
				var main = plus.android.runtimeMainActivity();
				var obj = main.getContentResolver();
				plus.android.importClass(obj);

				var contentValues = plus.android.newObject("android.content.ContentValues");
				// plus.android.invoke(contentValues,"put",CallLog.Calls.CACHED_NAME,"name");
				plus.android.invoke(contentValues, "put", CallLog.Calls.NUMBER, number);
				plus.android.invoke(contentValues, "put", CallLog.Calls.DATE, Date.parse(callDate));
				plus.android.invoke(contentValues, "put", CallLog.Calls.DURATION, callDuration);
				plus.android.invoke(contentValues, "put", CallLog.Calls.TYPE, callType);
				// plus.android.invoke(contentValues,"put",CallLog.Calls.INCOMING_TYPE ,0);
				plus.android.invoke(contentValues, "put", CallLog.Calls.NEW, callNew);

				// console.log("contentValues",plus.android.invoke(contentValues,"get",CallLog.Calls.NUMBER))
				obj.insert(CallLog.Calls.CONTENT_URI, contentValues);
			},
			//查询通话记录
			queryCallLog() {
				var CallLog = plus.android.importClass('android.provider.CallLog');
				var main = plus.android.runtimeMainActivity();
				var obj = main.getContentResolver();
				plus.android.importClass(obj);
				var cursor = obj.query(CallLog.Calls.CONTENT_URI, null, null, null, null);
				plus.android.importClass(cursor);
				// var String = plus.android.importClass("java.lang.String");
				var content = [];
				var count = 0;

				if (cursor.moveToFirst()) {
					while (cursor.moveToNext()) {
						count++;
						//号码
						var number = cursor.getString(cursor.getColumnIndex(CallLog.Calls.NUMBER));
						//时间
						var date = new Date(parseInt(cursor.getString(cursor.getColumnIndexOrThrow(CallLog.Calls.DATE))));
						//联系人
						var contactName = cursor.getString(cursor.getColumnIndexOrThrow(CallLog.Calls.CACHED_NAME));

						// console.log("number",number,date);
						content.push({
							contactName: contactName,
							number: number,
							date: date
						});
					}
				}
				console.log("content", content);
			}
			// #endif
		}
	}
</script>

<style>
	.grayBorder {
		border: 1px solid #999999;
	}
</style>
