<template>
<div id="rules" class="modal" tabindex="-1" role="dialog">
  <div class="modal-dialog modal-lg" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Rules</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div id = "ruleList" class="modal-body">
        <span class = "badge badge-info">Objective</span>
        <p>You are trapped at the top of the Maths Tower. It is your mission to escape within 2.5 minutes!</p>
        <span class = "badge badge-info">How to Play</span>
        <p>There are 10 floors (of course!) and each floor requires a number of keys to collect to move down to the next floor.
            For each question you solve correctly, you will gain a key otherwise your energy depletes.
            The colour of the energy bar and timer bar will change at midpoint and when running extremely low. Keep an eye on it!
        </p>
        <p>The game is over when the time runs out or when your energy is fully depleted.</p>
        <span class = "badge badge-info">Controls</span>
        <p>Type your answer and press enter/click Submit.</p>
        <img style="width:100%" src="../assets/img/game.png">

      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>   
</template>

<style>
    #ruleList {
        text-align:left;
    }
</style>