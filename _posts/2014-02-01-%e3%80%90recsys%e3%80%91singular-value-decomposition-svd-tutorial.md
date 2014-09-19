---
title: 【RecSys】Singular Value Decomposition (SVD) Tutorial
author: Flowerowl
layout: post
permalink: /3355.html
views:
  - 204
duoshuo_thread_id:
  - 1220743779864322559
categories:
  - 推荐系统系列
tags:
  - recsys
  - svd
---
<span style="font-size: 16px;"><span style="color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">When you browse standard web sources like </span><a style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #0066cc; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" href="http://en.wikipedia.org/wiki/Singular_value_decomposition">Singular Value Decomposition (SVD) on Wikipedia</a><span style="color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">, you find many equations, but not an intuitive explanation of what it is or how it works. Singular Value Decomposition is a way of factoring matrices into a series of linear approximations that expose the underlying structure of the matrix.</span></span>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">SVD is extraordinarily useful and has many applications such as data analysis, signal processing, pattern recognition, image compression, weather prediction, and Latent Semantic Analysis or LSA (also referred to as Latent Semantic Indexing or LSI).</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px; color: #679a00; line-height: 24px;">How does SVD work?</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">As a simple example, let&#8217;s look at golf scores. Suppose Phil, Tiger, and Vijay play together for 9 holes and they each make par on every hole. Their scorecard, which can also be viewed as a (hole x player) matrix might look like this.</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="1">
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">Hole</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Par</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Phil</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Tiger</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Vijay</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">1</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">2</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">6</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">7</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">8</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">9</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Let&#8217;s look at the problem of trying to predict what score each player will make on a given hole. One idea is give each hole a HoleDifficulty factor, and each player a PlayerAbility factor. The actual score is predicted by multiplying these two factors together.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">PredictedScore = HoleDifficulty * PlayerAbility</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">For the first attempt, let&#8217;s make the HoleDifficulty be the par score for the hole, and let&#8217;s make the player ability equal to 1. So on the first hole, which is par 4, we would expect a player of ability 1 to get a score of 4.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">PredictedScore = HoleDifficulty * PlayerAbility = 4 * 1 = 4</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">For our entire scorecard or matrix, all we have to do is multiply the PlayerAbility (assumed to be 1 for all players) by the HoleDifficulty (ranges from par 3 to par 5) and we can exactly predict all the scores in our example.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">In fact, this is the one dimensional (1-D) SVD factorization of the scorecard. We can represent our scorecard or matrix as the product of two vectors, the HoleDifficulty vector and the PlayerAbility vector. To predict any score, simply multiply the appropriate HoleDifficulty factor by the appropriate PlayerAbility factor. Following normal vector multiplication rules, we can generate the matrix of scores by multiplying the HoleDifficulty vector by the PlayerAbility vector, according to the following equation.</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">HoleDifficulty</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">1</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Mathematicians like to keep everything orderly, so the convention is that all vectors should be scaled so they have length 1. For example, the PlayerAbility vector is modified so that the sum of the squares of its elements add to 1, instead of the current 1<span style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: super;"><small style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline;">2</small></span> + 1<span style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: super;"><small style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline;">2</small></span> + 1<span style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: super;"><small style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline;">2</small></span>= 3. To do this, we have to divide each element by the square root of 3, so that when we square it, it becomes 1/3 and the three elements add to 1. Similarly, we have to divide each HoleDifficulty element by the square root of 148. The square root of 3 times the square root of 148 is our scaling factor 21.07. The complete 1-D SVD factorization (to 2 decimal places) is:</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">HoleDifficulty</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.41</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.25</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.25</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.41</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">ScaleFactor</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">21.07</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.58</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.58</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.58</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Our HoleDifficulty vector, that starts with 0.33, is called the Left Singular Vector. The ScaleFactor is the Singular Value, and our PlayerAbility vector, that starts with 0.58 is the Right Singular Vector. If we represent these 3 parts exactly, and multiply them together, we get the exact original scores. This means our matrix is a rank 1 matrix, another way of saying it has a simple and predictable pattern.</span>
</p>

<h2 style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 1.6em; vertical-align: baseline; line-height: 24px; color: #679a00; font-family: Helvetica, Arial, Geneva, sans-serif;">
  <span style="font-size: 16px;">Extending the SVD with More Factors</span>
</h2>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">More complicated matrices cannot be completely predicted just by using one set of factors as we have done. In that case, we have to introduce a second set of factors to refine our predictions. To do that, we subtract our predicted scores from the actual scores, getting the residual scores. Then we find a second set of HoleDifficulty2 and PlayerAbility2 numbers that best predict the residual scores.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Rather than guessing HoleDifficulty and PlayerAbility factors and subtracting predicted scores, there exist powerful algorithms than can calculate SVD factorizations for you. Let&#8217;s look at the actual scores from the first 9 holes of the 2007 Players Championship as played by Phil, Tiger, and Vijay.</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="1">
  <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">Hole</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Par</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Phil</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Tiger</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">Vijay</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">1</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">2</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">2</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">6</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">7</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">8</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">3</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">2</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">4</span>
    </td>
  </tr>
  
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;">9</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
    
    <td>
      <span style="font-size: 16px;">5</span>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">The 1-D SVD factorization of the scores is shown below. To make this example easier to understand, I have incorporated the ScaleFactor into the PlayerAbility and HoleDifficulty vectors so we can ignore the ScaleFactor for this example.</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.95</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.64</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.34</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.27</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5.02</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.69</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">2.42</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">2.85</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">2.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.97</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.67</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.36</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.64</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.28</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.00</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.69</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4.05</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3.92</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.08</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3.63</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3.39</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.55</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5.35</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5.00</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">HoleDifficulty</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.34</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.69</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">2.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.36</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.00</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.05</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.39</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5.00</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.91</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1.07</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1.00</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Notice that the HoleDifficulty factor is almost the average of that hole for the 3 players. For example hole 5, where everyone scored 4, does have a factor of 4.00. However hole 6, where the average score is also 4, has a factor of 4.05 instead of 4.00. Similarly, the PlayerAbility is almost the percentage of par that the player achieved, For example Tiger shot 39 with par being 36, and 39/36 = 1.08 which is almost his PlayerAbility factor (for these 9 holes) of 1.07.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Why don&#8217;t the hole averages and par percentages exactly match the 1-D SVD factors? The answer is that SVD further refines those numbers in a cycle. For example, we can start by assuming HoleDifficulty is the hole average and then ask what PlayerAbility best matches the scores, given those HoleDifficulty numbers? Once we have that answer we can go back and ask what HoleDifficulty best matches the scores given those PlayerAbility numbers? We keep iterating this way until we converge to a set of factors that best predict the score. SVD shortcuts this process and immediately give us the factors that we would have converged to if we carried out the process.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">One very useful property of SVD is that it always finds the optimal set of factors that best predict the scores, according to the standard matrix similarity measure (the Frobenius norm). That is, if we use SVD to find the factors of a matrix, those are the best factors that can be found. This optimality property means that we don&#8217;t have to wonder if a different set of numbers might predict scores better.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Now let&#8217;s look at the difference between the actual scores and our 1-D approximation. A plus difference means that the actual score is higher than the predicted score, a minus difference means the actual score is lower than the prediction. For example, on the first hole Tiger got a 4 and the predicted score was 4.64 so we get 4 &#8211; 4.64 = -0.64. In other words, we must add -0.64 to our prediction to get the actual score.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">Once these differences have been found, we can do the same thing again and predict these differences using the formula HoleDifficulty2 * PlayerAbility2. Since these factors are trying to predict the differences, they are the 2-D factors and we have put a 2 after their names (ex. HoleDifficulty2) to show they are the second set of factors.</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.05</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.64</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.28</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.02</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.31</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.58</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.15</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.03</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.36</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.36</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.28</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.00</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.69</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.67</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.05</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.67</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.08</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.66</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-1.08</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.37</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.61</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.45</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.35</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.00</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">HoleDifficulty2</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.18</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.38</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.80</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.15</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.35</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.67</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.89</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-1.29</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.44</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility2</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.82</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.20</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.53</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">There are some interesting observations we can make about these factors. Notice that hole 8 has the most significant HoleDifficulty2 factor (-1.29). That means that it is the hardest hole to predict. Indeed, it was the only hole on which none of the 3 players made par. It was especially hard to predict because it was the most difficult hole relative to par (HoleDifficulty &#8211; par) = (3.39 &#8211; 3) = 0.39, and yet Phil birdied it making his score more than a stroke below his predicted score (he scored 2 versus his predicted score of 3.08). Other holes that were hard to predict were holes 3 (0.80) and 7 (0.89) because Vijay beat Phil on those holes even though, in general, Phil was playing better.</span>
</p>

<h2 style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 1.6em; vertical-align: baseline; line-height: 24px; color: #679a00; font-family: Helvetica, Arial, Geneva, sans-serif;">
  <span style="font-size: 16px;">The Full SVD Factorization</span>
</h2>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">The full SVD for this example matrix (9 holes by 3 players) has 3 sets of factors. In general, a m x n matrix where m >= n can have at most n factors, so our 9 x 3 matrix cannot have more than 3 sets of factors. Here is the full SVD factorization (to two decimal places).</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">2</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">2</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">HoleDifficulty 1-3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.34</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.18</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.90</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.69</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.38</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.15</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">2.66</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.80</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.40</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.36</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.15</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.47</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.00</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.35</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.29</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4.05</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.67</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.68</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.66</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.89</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3.39</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-1.29</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.14</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5.00</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.44</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.36</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility 1-3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.91</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1.07</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1.00</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.82</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.20</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.53</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.21</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.76</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.62</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">By SVD convention, the HoleDifficulty and PlayerAbility vectors should all have length 1, so the conventional SVD factorization is:</span>
</p>

<table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;" border="0">
  <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
    <td>
      <span style="font-size: 16px;"> </span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">2</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">2</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">4</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">5</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">=</span>
    </td>
    
    <td>
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">HoleDifficulty 1-3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.35</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.09</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.64</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.38</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.19</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.10</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.22</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.40</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.28</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.36</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.08</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.18</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.20</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.33</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.48</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.30</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.44</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.23</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.28</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.64</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.10</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.41</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.22</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.25</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">ScaleFactor 1-3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">21.07</span>
          </td>
          
          <td>
            <span style="font-size: 16px;"></span>
          </td>
          
          <td>
            <span style="font-size: 16px;"></span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;"></span>
          </td>
          
          <td>
            <span style="font-size: 16px;">2.01</span>
          </td>
          
          <td>
            <span style="font-size: 16px;"></span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;"></span>
          </td>
          
          <td>
            <span style="font-size: 16px;"></span>
          </td>
          
          <td>
            <span style="font-size: 16px;">1.42</span>
          </td>
        </tr>
      </table>
    </td>
    
    <td valign="middle">
      <span style="font-size: 16px;">*</span>
    </td>
    
    <td valign="middle">
      <table style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; border-collapse: collapse; border-spacing: 0px;" border="1">
        <caption style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;"><span style="font-size: 16px;"> </span></caption> <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td colspan="3">
            <span style="font-size: 16px;">PlayerAbility 1-3</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">Phil</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Tiger</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">Vijay</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">0.53</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.62</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.58</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.82</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.20</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.53</span>
          </td>
        </tr>
        
        <tr style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline;">
          <td>
            <span style="font-size: 16px;">-0.21</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">0.76</span>
          </td>
          
          <td>
            <span style="font-size: 16px;">-0.62</span>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;"> </span>
</p>

<h2 style="margin: 0px; padding: 0px; border: 0px; outline: 0px; font-size: 1.6em; vertical-align: baseline; line-height: 24px; color: #679a00; font-family: Helvetica, Arial, Geneva, sans-serif;">
  <span style="font-size: 16px;">Latent Semantic Analysis Tutorial</span>
</h2>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">We hope that you have some idea of what SVD is and how it can be used. Next we&#8217;ll cover how SVD is used in our <a style="margin: 0px; padding: 0px; border: 0px; outline: 0px; vertical-align: baseline; color: #0066cc;" href="http://www.puffinwarellc.com/p3b.htm">Latent Semantic Analysis Tutorial</a>. Although the domain is different, the concepts are the same. We are trying to predict patterns of how words occur in documents instead of trying to predict patterns of how players score on golf holes.</span>
</p>

<p style="margin: 1em 0px; padding: 0px; border: 0px; outline: 0px; font-size: 12px; vertical-align: baseline; color: #666666; font-family: Helvetica, Arial, Geneva, sans-serif; line-height: 17.02083396911621px;">
  <span style="font-size: 16px;">转载自：<a href="http://www.puffinwarellc.com/index.php/news-and-articles/articles/30-singular-value-decomposition-tutorial.html">http://www.puffinwarellc.com/index.php/news-and-articles/articles/30-singular-value-decomposition-tutorial.html</a></span>
</p>

转载请注明：[于哲的博客][1] &raquo; [【RecSys】Singular Value Decomposition (SVD) Tutorial][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3355.html