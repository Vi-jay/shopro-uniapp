
<script>
	import {
		init
	} from "@/shopro";
	export default {
		async onLaunch(options) {
			// #ifdef APP-PLUS
			uni.showLoading({
				mask: true
			});
			await new Promise(function(resolve) {
				plus.runtime.getProperty(plus.runtime.appid, function(widgetInfo) {
					uni.request({
						url: 'https://h5.fulugame.cn/testApi/api/download',
						data: {
							version: widgetInfo.version,
							name: 'kongfu'
						},
						success: (result) => {
							var data = result.data;
							if(!data.update)return resolve();
							if (data.update && data.wgtUrl) {
								uni.downloadFile({
									url: data.wgtUrl,
									success: (downloadResult) => {
										if (downloadResult.statusCode ===
											200) {
											plus.runtime.install(downloadResult
												.tempFilePath, {
													force: true
												},
												function() {
													console.log(
														'install success...'
														);
													plus.runtime.restart();
												},
												function(e) {
													console.error(
														'install fail...'
														);
														plus.runtime.restart();
												});
										}
									}
								});
							}
						}
					});
				});
			});
			uni.hideLoading();
			// #endif
			init(options);
		},
	};
</script>

<style lang="scss">
	@import 'static/style/index.scss';
</style>
