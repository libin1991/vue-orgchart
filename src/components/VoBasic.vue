<template>
<div id="chart-container" class="vo-basic"></div>
</template>

<script>
import OrgChart from '../lib/orgchart'
import { mergeOptions } from '../lib/lodash.js'
export default {
  name: 'orgchart',
  props: {
    data: { type: Object, default () { return {} } },
    pan: { type: Boolean, default: false },
    zoom: { type: Boolean, default: false },
    direction: { type: String, default: 't2b' },
    verticalDepth: { type: Number },
    toggleSiblingsResp: { type: Boolean, default: false },
    ajaxURL: { type: Object },
    depth: { type: Number, default: 999 },
    nodeTitle: { type: String, default: 'name' },
    parentNodeSymbol: { type: String, default: 'fa-users' },
    nodeContent: { type: String },
    nodeId: { type: String, default: 'id' },
    createNode: { type: Function },
    exportButton: { type: Boolean, default: false },
    exportFilename: { type: String },
    chartClass: { type: String, default: '' },
    draggable: { type: Boolean, default: false },
    dropCriteria: { type: Function }
  },
  data () {
    return {
      orgchart: null,
      defaultOptions: {
        'chartContainer': '#chart-container'
      }
    }
  },
  mounted() {
    this.initOrgChart()
  },
  methods: {
    initOrgChart() {
      const opts = mergeOptions(this.defaultOptions, this.$props)
      this.orgchart = new OrgChart(opts)
    }
  }
}
</script>

<style>
#wrapper {
  width: 50%;
  margin: 0 auto;
}

#wrapper li {
  margin-top: 20px;
}

#wrapper a {
  font-size: 24px;
}

#wrapper span {
  font-size: 24px;
}

#chart-container {
  position: relative;
  display: inline-block;
  top: 10px;
  left: 10px;
  height: 50%;
  width: calc(100% - 24px);
  border-radius: 5px;
  overflow: auto;
  overflow-x: hidden;
  text-align: center;
  font-family: "Source Sans Pro", "Helvetica Neue", Arial, sans-serif;
  font-size: 14px;
}
.orgchart {
  display: inline-block;
  min-height: 100%;
  min-width: 100%;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-size: 10px 10px;
  border: 1px dashed transparent;
}

.orgchart .hidden, .orgchart~.hidden {
  display: none;
}

.orgchart div,
.orgchart div::before,
.orgchart div::after {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.orgchart.b2t {
  -ms-transform: rotate(180deg);
  -moz-transform: rotate(180deg);
  -webkit-transform: rotate(180deg);
  transform: rotate(180deg);
}

.orgchart.l2r {
  position: absolute;
  -ms-transform: rotate(-90deg) rotateY(180deg);
  -moz-transform: rotate(-90deg) rotateY(180deg);
  -webkit-transform: rotate(-90deg) rotateY(180deg);
  transform: rotate(-90deg) rotateY(180deg);
  -ms-transform-origin: left top;
  -moz-transform-origin: left top;
  -webkit-transform-origin: left top;
  transform-origin: left top;
}

.orgchart .verticalNodes ul {
  list-style: none;
  margin: 0;
  padding-left: 18px;
  text-align: left;
}

.orgchart .verticalNodes ul:first-child {
  margin-top: 3px;
}

.orgchart .verticalNodes>td::before {
  content: '';
  border: 1px solid rgba(217, 83, 79, 0.8);
}

.orgchart .verticalNodes>td>ul>li:first-child::before {
  top: -4px;
  height: 30px;
  width: calc(50% - 2px);
  border-width: 2px 0 0 2px;
}

.orgchart .verticalNodes ul>li {
  position: relative;
}

.orgchart .verticalNodes ul>li::before,
.orgchart .verticalNodes ul>li::after {
  content: '';
  position: absolute;
  left: -6px;
  border-color: rgba(217, 83, 79, 0.8);
  border-style: solid;
  border-width: 0 0 2px 2px;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

.orgchart .verticalNodes ul>li::before {
  top: -4px;
  height: 30px;
  width: 11px;
}

.orgchart .verticalNodes ul>li::after {
  top: 1px;
  height: 100%;
}

.orgchart .verticalNodes ul>li:first-child::after {
  top: 24px;
  width: 11px;
  border-width: 2px 0 0 2px;
}

.orgchart .verticalNodes ul>li:last-child::after {
  border-width: 2px 0 0;
}

.orgchart.r2l {
  position: absolute;
  -ms-transform: rotate(90deg);
  -moz-transform: rotate(90deg);
  -webkit-transform: rotate(90deg);
  transform: rotate(90deg);
  -ms-transform-origin: left top;
  -moz-transform-origin: left top;
  -webkit-transform-origin: left top;
  transform-origin: left top;
}

.orgchart>.spinner {
  font-size: 100px;
  margin-top: 30px;
  color: rgba(68, 157, 68, 0.8);
}

.orgchart table {
  border-spacing: 0;
  border-collapse: separate;
}

.orgchart>table:first-child{
  margin: 20px auto;
}

.orgchart td {
  text-align: center;
  vertical-align: top;
  padding: 0;
}

.orgchart tr.lines .topLine {
  border-top: 2px solid #616161;
}

.orgchart tr.lines .rightLine {
  border-right: 1px solid #616161;
  float: none;
  border-radius: 0;
}

.orgchart tr.lines .leftLine {
  border-left: 1px solid #616161;
  float: none;
  border-radius: 0;
}

.orgchart tr.lines .downLine {
  background-color: #616161;
  margin: 0 auto;
  height: 20px;
  width: 2px;
  float: none;
}

/* node styling */
.orgchart .node {
  display: inline-block;
  position: relative;
  margin: 0;
  padding: 3px;
  border: 2px dashed transparent;
  text-align: center;
  width: 130px;
}

.orgchart.l2r .node, .orgchart.r2l .node {
  width: 50px;
  height: 130px;
}

.orgchart .node>.hazy {
  opacity: 0.2;
}

.orgchart .node>.spinner {
  position: absolute;
  top: calc(50% - 15px);
  left: calc(50% - 15px);
  vertical-align: middle;
  font-size: 30px;
  color: rgba(68, 157, 68, 0.8);
}

.orgchart .node:hover {
  background-color: rgba(238, 217, 54, 0.5);
  transition: .5s;
  cursor: default;
  z-index: 20;
}

.orgchart .node.focused {
  background-color: rgba(238, 217, 54, 0.5);
}

.orgchart .ghost-node {
  position: fixed;
  left: -10000px;
  top: -10000px;
}

.orgchart .ghost-node rect {
  fill: #ffffff;
  stroke: #bf0000;
}

.orgchart .node.allowedDrop {
  border-color: rgba(68, 157, 68, 0.9);
}

.orgchart .node .title {
  text-align: center;
  font-size: 12px;
  font-weight: 300;
  height: 20px;
  line-height: 20px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  background-color: #42b983;
  color: #fff;
  border-radius: 4px 4px 0 0;
}

.orgchart.b2t .node .title {
  -ms-transform: rotate(-180deg);
  -moz-transform: rotate(-180deg);
  -webkit-transform: rotate(-180deg);
  transform: rotate(-180deg);
  -ms-transform-origin: center bottom;
  -moz-transform-origin: center bottom;
  -webkit-transform-origin: center bottom;
  transform-origin: center bottom;
}

.orgchart.l2r .node .title {
  -ms-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -moz-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -webkit-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -ms-transform-origin: bottom center;
  -moz-transform-origin: bottom center;
  -webkit-transform-origin: bottom center;
  transform-origin: bottom center;
  width: 120px;
}

.orgchart.r2l .node .title {
  -ms-transform: rotate(-90deg) translate(-40px, -40px);
  -moz-transform: rotate(-90deg) translate(-40px, -40px);
  -webkit-transform: rotate(-90deg) translate(-40px, -40px);
  transform: rotate(-90deg) translate(-40px, -40px);
  -ms-transform-origin: bottom center;
  -moz-transform-origin: bottom center;
  -webkit-transform-origin: bottom center;
  transform-origin: bottom center;
  width: 120px;
}

.orgchart .node .title .symbol {
  float: left;
  margin-top: 4px;
  margin-left: 2px;
}

.orgchart .node .content {
  width: 100%;
  height: 20px;
  font-size: 11px;
  line-height: 18px;
  border: 1px solid #42b983;
  border-radius: 0 0 4px 4px;
  text-align: center;
  background-color: #fff;
  color: #333;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.orgchart.b2t .node .content {
  -ms-transform: rotate(180deg);
  -moz-transform: rotate(180deg);
  -webkit-transform: rotate(180deg);
  transform: rotate(180deg);
  -ms-transform-origin: center top;
  -moz-transform-origin: center top;
  -webkit-transform-origin: center top;
  transform-origin: center top;
}

.orgchart.l2r .node .content {
  -ms-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -moz-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -webkit-transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  transform: rotate(-90deg) translate(-40px, -40px) rotateY(180deg);
  -ms-transform-origin: top center;
  -moz-transform-origin: top center;
  -webkit-transform-origin: top center;
  transform-origin: top center;
  width: 120px;
}

.orgchart.r2l .node .content {
  -ms-transform: rotate(-90deg) translate(-40px, -40px);
  -moz-transform: rotate(-90deg) translate(-40px, -40px);
  -webkit-transform: rotate(-90deg) translate(-40px, -40px);
  transform: rotate(-90deg) translate(-40px, -40px);
  -ms-transform-origin: top center;
  -moz-transform-origin: top center;
  -webkit-transform-origin: top center;
  transform-origin: top center;
  width: 120px;
}

.orgchart .node .edge {
  font-size: 15px;
  position: absolute;
  color: rgba(68, 157, 68, 0.5);
  cursor: default;
  transition: .2s;
  -webkit-transition: .2s;
}

.orgchart.noncollapsable .node .edge {
  display: none;
}

.orgchart .edge:hover {
  color: #449d44;
  cursor: pointer;
}

.orgchart .node .verticalEdge {
  width: calc(100% - 10px);
  width: -webkit-calc(100% - 10px);
  width: -moz-calc(100% - 10px);
  left: 5px;
}

.orgchart .node .topEdge {
  top: -4px;
}

.orgchart .node .bottomEdge {
  bottom: -4px;
}

.orgchart .node .horizontalEdge {
  width: 15px;
  height: calc(100% - 10px);
  height: -webkit-calc(100% - 10px);
  height: -moz-calc(100% - 10px);
  top: 5px;
}

.orgchart .node .rightEdge {
  right: -4px;
}

.orgchart .node .leftEdge {
  left: -4px;
}

.orgchart .node .horizontalEdge::before {
  position: absolute;
  top: calc(50% - 7px);
  top: -webkit-calc(50% - 7px);
  top: -moz-calc(50% - 7px);
}

.orgchart .node .rightEdge::before {
  right: 3px;
}

.orgchart .node .leftEdge::before {
  left: 3px;
}

.orgchart .node .toggleBtn {
  position: absolute;
  left: 5px;
  bottom: -2px;
  color: rgba(68, 157, 68, 0.6);
}

.orgchart .node .toggleBtn:hover {
  color: rgba(68, 157, 68, 0.8);
}

.oc-export-btn {
  display: inline-block;
  position: absolute;
  right: 5px;
  bottom: 5px;
  padding: 6px 12px;
  margin-bottom: 0;
  font-size: 14px;
  font-weight: 400;
  line-height: 1.42857143;
  text-align: center;
  white-space: nowrap;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  color: #fff;
  background-color: #409eff;
  border: 1px solid transparent;
  border-color: #409eff;
  border-radius: 4px;
}

.oc-export-btn:hover,.oc-export-btn:focus,.oc-export-btn:active  {
  background-color: #409eff;
  border-color: #409eff;
}

.orgchart~.mask {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 999;
  text-align: center;
  background-color: rgba(0,0,0,0.3);
}

.orgchart~.mask .spinner {
  position: absolute;
  top: calc(50% - 54px);
  left: calc(50% - 54px);
  color: rgba(255,255,255,0.8);
  font-size: 108px;
}

.orgchart .node {
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  top: 0;
  left: 0;
}

.orgchart .slide-down {
  opacity: 0;
  top: 40px;
}

.orgchart.l2r .node.slide-down, .orgchart.r2l .node.slide-down {
  top: 130px;
}

.orgchart .slide-up {
  opacity: 0;
  top: -40px;
}

.orgchart.l2r .node.slide-up, .orgchart.r2l .node.slide-up {
  top: -130px;
}

.orgchart .slide-right {
  opacity: 0;
  left: 130px;
}

.orgchart.l2r .node.slide-right, .orgchart.r2l .node.slide-right {
  left: 40px;
}

.orgchart .slide-left {
  opacity: 0;
  left: -130px;
}

.orgchart.l2r .node.slide-left, .orgchart.r2l .node.slide-left {
  left: -40px;
}
#edit-panel {
  position: relative;
  left: 10px;
  width: calc(100% - 40px);
  border-radius: 4px;
  float: left;
  margin-top: 20px;
  padding: 10px 5px 10px 10px;
  font-size: 14px;
  line-height: 1.5;
  border-radius: 2px;
  color: rgba(0, 0, 0, 0.65);
  background-color: #fff;
}

#edit-panel .btn-inputs {
  font-size: 24px;
}

#edit-panel label {
  font-weight: bold;
}

#edit-panel .new-node {
  height: 24px;
  -webkit-appearance: none;
  background-color:#fff;
  background-image: none;
  border-radius: 4px;
  line-height: 1;
  border: 1px solid #d8dce5;
  box-sizing: border-box;
  color: #5a5e66;
  display: inline-block;
  transition: border-color .2s cubic-bezier(.645,.045,.355,1);
}

#edit-panel .new-node:focus {
  border-color: #409eff;
  outline: none;
}

#edit-panel .new-node:hover {
  border-color: #b4bccc;
}

#edit-panel.edit-parent-node .selected-node-group{
  display: none;
}

#chart-state-panel, #selected-node, #btn-remove-input {
  margin-right: 20px;
}

#edit-panel button {
  /* color: #333;
  background-color: #fff; */
  display: inline-block;
  padding: 6px 12px;
  margin-bottom: 0;
  line-height: 1.42857143;
  text-align: center;
  white-space: nowrap;
  border-radius: 4px;
  vertical-align: middle;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  background-image: none;
}

#edit-panel.edit-parent-node button:not(#btn-add-nodes) {
  display: none;
}

#edit-panel button:hover,.edit-panel button:focus,.edit-panel button:active {
  border-color: #409eff;
  box-shadow:  0 0 10px #409eff;
}

#new-nodelist {
  display: inline-block;
  list-style:none;
  margin-top: -2px;
  padding: 0;
  vertical-align: text-top;
}

#new-nodelist>* {
  padding-bottom: 4px;
}

.btn-inputs {
  vertical-align: sub;
}


.btn-inputs:hover {
  text-shadow: 0 0 4px #fff;
}

.radio-panel input[type='radio'] {
  border: 1px solid #d8dce5;
  border-radius: 100%;
  cursor: pointer;
  box-sizing: border-box;
  display: inline-block;
  height: 14px;
  width: 14px;
  vertical-align: top;
}

#edit-panel.view-state .radio-panel input[type='radio']+label {
  font-size: 14px;
  font-weight: 500;
  /* color: #5a5e66; */
  line-height: 1;
}

#btn-add-nodes {
  margin-left: 20px;
}
</style>
