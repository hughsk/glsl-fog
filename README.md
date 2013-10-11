# glsl-fog [![experimental](http://hughsk.github.io/stability-badges/dist/experimental.svg)](http://github.com/hughsk/stability-badges) #

Basic fog functions for GLSL, generic but intended for use with
[glslify](http://github.com/chrisdickinson/glslify).

## Usage ##

[![glsl-fog](https://nodei.co/npm/glsl-fog.png?mini=true)](https://nodei.co/npm/glsl-fog)

Here's a hypothetical example of linear fog calculated in a vertex shader:

``` glsl
#define FOG_START 100
#define FOG_END 500

varying float fogAmount;
uniform vec3 position;

#pragma glslify: fog_linear = require(glsl-fog/linear)

void main() {
  gl_Position = projection * view * model * vec4(positon, 1.0);
  float fogDistance = length(gl_Position.xyz);
  fogAmount = fog_linear(fogDistance, FOG_START, FOG_END);
}
```

And another (separate) example using exp/exp2 per-pixel fog:

``` glsl
#define FOG_DENSITY 0.05

varying vec4 vertexColor;

// Take your pick: these should be usable interchangeably.
#pragma glslify: fog_exp2 = require(glsl-fog/exp2)
#pragma glslify: fog_exp = require(glsl-fog/exp)

void main() {
  float fogDistance = gl_FragCoord.z / gl_FragCoord.w;
  float fogAmount = fog_exp2(fogDistance, FOG_DENSITY);
  const float fogColor = vec4(1.0); // white

  gl_FragColor = mix(vertexColor, fogColor, fogAmount);
}
```

## License ##

MIT. See [LICENSE.md](http://github.com/hughsk/glsl-fog/blob/master/LICENSE.md) for details.
