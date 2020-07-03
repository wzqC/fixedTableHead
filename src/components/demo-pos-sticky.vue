<template>
  <div class="table">
		<!-- 表头 -->
		<div class="table-top">
			<div class="table-top-left leftFix">
				<div class="name">姓名</div>
				<div class="chose">选择</div>
				<div class="times">计薪时长</div>
			</div>
			<div class="table-top-right">
				<ul :style="{transform: `translateX(${-scrollLeft}px)`}">
					<li v-for="(item, i) in classList" :key="i">{{item.name}}</li>
				</ul>
			</div>
		</div>

		<div id="table-bottom" class="table-bottom" @scroll="onScroll">
			<!-- 左边固定列 -->
			<div class="table-bottom-left">
				<ul>
					<li v-for="(item, i) in editList" :key="i">
						<div class="name" :class="{'firstItem': i === 0}">test{{i}}</div>
						<div class="chose" :class="{'firstItem': i === 0}">
							<input type="radio">
						</div>
						<div class="times" :class="{'firstItem': i === 0}">{{i}}</div>
					</li>
				</ul>
			</div>
			<!-- 可编辑表格 -->
			<div class="table-main">
				<ul v-for="(courseAll, i) in editList" :key="i">
					<li v-for="(cell, j) in courseAll" :key="j">
						{{cell.courseName.substr(0, 1)}}
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import {courseList, classList} from './mock.js'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
	},
	data () {
		return {
			courseList,
			classList,
			scrollLeft: 0,
			scrollTop: 0,
		}
	},
	computed: {
		// 根据表头和表列生成可编辑表格
		editList () {
			let editList = []
			editList = courseList.map(course => {
				return classList.map(varClass => {
					return {
						courseId: course.id,
						courseName: course.name,
						classId: varClass.id,
						className: varClass.name,
					}
				})
			})
			return editList
		}
	},
	mounted() {
		document.getElementById('table-bottom').scrollLeft = 300
	},
	methods: {
		onScroll (e) {
			let {scrollTop, scrollLeft} = e.target
			this.scrollLeft = scrollLeft
		},
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
@headWidth: 170px;
@headHeight: 28px;
@headBackground: #F3F4F6;
@cellWidth: 40px;
@cellHeight: 40px;

li, .table-top-left {
	// display: flex;
	// justify-content: space-around;
	// align-items: center;
}

.name{
	width:60px;
	border-left: 1px solid #ddd;
}
.chose{
	width:40px;
	border-left: 1px solid #ddd;
	border-right: 1px solid #ddd;
}
.times{
	width:70px;
	border-right: 1px solid #ddd;
}

.table {
	width: 100%;
	height: 600px;
	display: flex;
	flex-direction: column;
	margin-top:30px;
	.table-top {
		display: flex;
		.table-top-left {
			width: @headWidth;
			min-width: @headWidth;
			height: @headHeight;
			line-height:  @headHeight;
			background: @headBackground;
			font-size: 12px;
			> div{
				float:left;
				height:100%;
				box-sizing: border-box;
			}
		}
		.table-top-right {
			overflow: hidden;
			>ul {
				height:  @headHeight;
				width: auto;
				white-space: nowrap;
				>li {
					width: 80px;
					height: @headHeight;
					background: #fff;
					display: inline-block;
					box-sizing: border-box;
					border-right:1px solid #ddd;
					border-bottom: 1px solid #ddd;
				}
			}
		}
	}
	.table-bottom {
		flex: 1;
		overflow: auto;
		display: flex;
		position: relative;
		.table-bottom-left {
			position: sticky;
			left: 0;
			top: 0;
			height: fit-content;
			>ul {
				>li {
					width: @headWidth;
					height: @cellHeight;
					line-height: @cellHeight;
					background: #fff;
					font-size: 12px;
					> div{
						float:left;
						height:100%;
						box-sizing: border-box;
						border-bottom: 1px solid #ddd;
					}
					.firstItem{
						border-top: 1px solid #ddd;
					}
				}
			}
		}
		.table-main {
			>ul {
				height: 40px;
				width: auto;
				white-space: nowrap;
				>li {
					width: @cellWidth;
					height: @cellHeight;
					font-size: 12px;
					box-sizing: border-box;
					border-right: 1px solid #ddd;
					border-bottom: 1px solid #ddd;
					overflow: hidden;
					display: inline-block;
					text-align: center;
					line-height: 40px;
				}
			}
		}
	}
}
</style>
