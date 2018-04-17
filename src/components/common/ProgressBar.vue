<template>
  <div class="progress">
    <a href='#/'>
      <div class="circle active">
        <span class="label"></span>
        <span class="title">Profile</span>
      </div>
    </a>
    <span class="bar"></span>
    <a href='#/lab_exam'>
      <div class="circle">
        <span class="label"></span>
        <span class="title">Lab</span>
      </div>
    </a>
    <span class="bar"></span>
    <a href='#/suggestion'>
      <div class="circle">
        <span class="label"></span>
        <span class="title">Request</span>
      </div>
    </a>
  </div>
</template>

<script>

</script>

<style lang="scss">
  /* Form Progress */

  .progress {
    margin: 20px auto;
    text-align: center;
    .circle {
      display: inline-block;
      background: #fff;
      width: 28px;
      height: 28px;
      border-radius: 40px;
      border: 1px solid #d5d5da;
    }
    .bar {
      display: inline-block;
      background: #fff;
      width: 28px;
      height: 28px;
      border-radius: 40px;
      border: 1px solid #d5d5da;
      position: relative;
      width: 200px;
      height: 6px;
      top: -33px;
      margin-left: -5px;
      margin-right: -5px;
      border-left: none;
      border-right: none;
      border-radius: 0;
    }
    .circle {
      .label {
        display: inline-block;
        width: 20px;
        height: 20px;
        line-height: 32px;
        border-radius: 32px;
        margin-top: 3px;
        color: #b5b5ba;
        font-size: 17px;
      }
      .title {
        color: #b5b5ba;
        font-size: 13px;
        line-height: 30px;
      }
    }
    .bar.done, .circle.done {
      background: #eee;
    }
    .bar.active {
      background: linear-gradient(to right, #EEE 40%, #FFF 60%);
    }
    .circle {
      &.done {
        .label {
          color: #FFF;
          background: #8bc435;
          box-shadow: inset 0 0 2px rgba(0, 0, 0, 0.2);
        }
        .title {
          color: #444;
        }
      }
      &.active {
        .label {
          color: #FFF;
          background: #0c95be;
          box-shadow: inset 0 0 2px rgba(0, 0, 0, 0.2);
        }
        .title {
          color: #0c95be;
        }
      }
    }
  }
</style>
