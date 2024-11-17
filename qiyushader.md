a_indices;
attribute highp vec4 a_normal;
attribute highp vec3 a_position;
varying highp vec4 v_color0;
varying highp vec4 v_edgemap;
varying highp vec4 v_fog;
varying highp vec4 v_light;
varying highp vec2 v_texcoord0;
uniform highp mat4 u_view;
uniform highp mat4 u_proj;
uniform mat4 u_model[4];
uniform highp vec4 SubPixelOffset;
uniform highp vec4 OverlayColor;
uniform highp vec4 TileLightColor;
uniform highp vec4 FogColor;
uniform highp vec4 FogControl;
uniform mat4 Bones[8];
uniform highp vec4 ViewPositionAndTime;
void main ()
{
  highp vec4 fogColor_1;
  highp vec3 newFog_2;
  highp vec4 tmpvar_3;
  tmpvar_3.w = 1.0;
  tmpvar_3.xyz = a_position;
  highp vec4 tmpvar_4;
  highp mat4 offsetProj_5;
  offsetProj_5 = u_proj;
  highp vec4 tmpvar_6;
  tmpvar_6.yzw = u_proj[2].yzw;
  tmpvar_6.x = (u_proj[2].x + SubPixelOffset.x);
  offsetProj_5[2] = tmpvar_6;
  highp vec4 tmpvar_7;
  tmpvar_7.xzw = offsetProj_5[2].xzw;
  tmpvar_7.y = (offsetProj_5[2].y - SubPixelOffset.y);
  offsetProj_5[2] = tmpvar_7;
  highp vec4 tmpvar_8;
  tmpvar_8.w = 1.0;
  tmpvar_8.xyz = ((u_model[0] * Bones[int(a_indices)]) * tmpvar_3).xyz;
  tmpvar_4 = (offsetProj_5 * (u_view * tmpvar_8));
  v_texcoord0 = vec2(0.0, 0.0);
  v_color0 = vec4(0.0, 0.0, 0.0, 0.0);
  bool tmpvar_9;
  tmpvar_9 = ((FogColor.x == FogColor.z) && ((
    (FogColor.x - FogColor.y)
   > 0.24) || (
    (FogColor.y == 0.0)
   && 
    (FogColor.x > 0.1)
  )));
  bool tmpvar_10;
  highp float tmpvar_11;
  tmpvar_11 = (0.029 + ((0.09 * FogControl.y) * FogControl.y));
  bool tmpvar_12;
  if ((FogControl.x < 0.14)) {
    tmpvar_12 = (abs((FogControl.x - tmpvar_11)) < 0.02);
  } else {
    tmpvar_12 = bool(0);
  };
  bool tmpvar_13;
  tmpvar_13 = (FogControl.x == 0.0);
  tmpvar_10 = ((tmpvar_12 && (FogColor.x > 
    -(FogColor.y)
  )) || ((
    (tmpvar_13 && (FogColor.z == 0.0))
   && 
    (FogColor.y < 0.18)
  ) && (
    (FogColor.x - FogColor.y)
   > 0.1)));
  bool tmpvar_14;
  tmpvar_14 = ((tmpvar_13 && (FogControl.y < 0.8)) && ((FogColor.z > FogColor.x) || (FogColor.y > FogColor.x)));
  highp float tmpvar_15;
  highp vec2 tmpvar_16;
  tmpvar_16.y = 1.0;
  tmpvar_16.x = (0.5 + (20.0 / FogControl.z));
  highp vec2 tmpvar_17;
  tmpvar_17 = clamp (((FogControl.xy - tmpvar_16) / (vec2(0.23, 0.7) - tmpvar_16)), vec2(0.0, 0.0), vec2(1.0, 1.0));
  highp float tmpvar_18;
  tmpvar_18 = (tmpvar_17.x * tmpvar_17.y);
  tmpvar_15 = ((tmpvar_18 * tmpvar_18) * (3.0 - (2.0 * tmpvar_18)));
  if (tmpvar_14) {
    newFog_2 = ((vec3(1.8, 2.8, 1.8) * FogColor.xyz) * FogColor.xyz);
  } else {
    if (tmpvar_9) {
      newFog_2 = vec3(0.01, 0.0, 0.9);
    } else {
      highp vec3 factors_19;
      highp float tmpvar_20;
      tmpvar_20 = min (FogColor.y, 0.26);
      highp vec3 tmpvar_21;
      tmpvar_21.x = max ((FogColor.x * 0.6), max (FogColor.y, FogColor.z));
      tmpvar_21.y = (1.5 * max ((FogColor.x - FogColor.z), 0.0));
      tmpvar_21.z = tmpvar_20;
      factors_19.xy = tmpvar_21.xy;
      factors_19.z = (tmpvar_20 * tmpvar_20);
      highp vec3 horizonCol_22;
      horizonCol_22 = ((vec3(0.0, 0.5, 1.0) * (1.0 - FogColor.z)) + ((
        (((0.7 * tmpvar_21.x) * tmpvar_21.x) + (0.3 * tmpvar_21.x))
       + tmpvar_21.y) * vec3(5.7, 0.95, 0.0)));
      highp vec3 tmpvar_23;
      tmpvar_23 = mix (mix (horizonCol_22, (vec3(1.8, 7.0, 7.0) * tmpvar_21.x), (tmpvar_21.x * tmpvar_21.x)), (vec3(4.9, 12.348, 18.228) * factors_19.z), tmpvar_15);
      horizonCol_22 = tmpvar_23;
      highp float tmpvar_24;
      tmpvar_24 = (((2.1 * 
        (1.1 - FogColor.z)
      ) * FogColor.y) * (1.0 - tmpvar_15));
      newFog_2 = (tmpvar_23 * (vec3((1.0 - tmpvar_24)) + (vec3(0.3, 0.5, 0.0) * tmpvar_24)));
    };
  };
  highp float tmpvar_25;
  tmpvar_25 = (tmpvar_4.z / FogControl.z);
  fogColor_1.xyz = newFog_2;
  highp float tmpvar_26;
  highp float tmpvar_27;
  tmpvar_27 = clamp (((tmpvar_25 - FogControl.x) / (FogControl.y - FogControl.x)), 0.0, 1.0);
  tmpvar_26 = (tmpvar_27 * (tmpvar_27 * (3.0 - 
    (2.0 * tmpvar_27)
  )));
  fogColor_1.w = (tmpvar_26 + ((1.0 - tmpvar_26) * (0.3 - 
    (0.3 * exp(((
      -(tmpvar_25)
     * tmpvar_25) * (0.22 * 
      (19.0 - (18.0 * FogColor.y))
    ))))
  )));
  if (tmpvar_10) {
    highp vec3 tmpvar_28;
    tmpvar_28 = pow (FogColor.xyz, vec3(1.428571, 1.428571, 1.428571));
    fogColor_1.xyz = ((tmpvar_28 * (0.7966 + tmpvar_28)) / (0.7966 + (tmpvar_28 * 0.2034)));
  };
  highp vec3 light_29;
  highp float intensity_30;
  intensity_30 = ((0.7 + (0.3 * 
    abs(a_normal.y)
  )) * (0.9 + (0.1 * 
    abs(a_normal.x)
  )));
  intensity_30 = ((intensity_30 * 4.92) * (TileLightColor.z * TileLightColor.z));
  intensity_30 = (intensity_30 + (OverlayColor.w * 0.35));
  highp float tmpvar_31;
  tmpvar_31 = (TileLightColor.z - TileLightColor.x);
  highp vec3 tmpvar_32;
  tmpvar_32.z = 1.0;
  tmpvar_32.x = (1.0 - (2.8 * tmpvar_31));
  tmpvar_32.y = (1.0 - (2.7 * tmpvar_31));
  light_29 = ((intensity_30 * tmpvar_32) * (1.0 - (0.3 * 
    float((a_position.y >= 0.0))
  )));
 
