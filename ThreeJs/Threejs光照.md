## 绘制流程。
使用是：
1.创建light
2light到场景。
2.调用render方法。
### 解析render方法对light处理过程。
```javascript
this.render = function ( scene, camera, renderTarget, forceClear )
```
1. 会将确定当前需要绘制scene和camera。从而确定当前需要渲染哪些。其中场景决定了哪些灯光需要参与渲染。
2. render会调用renderState.setupLights(camera).
3. setupLights调用webGLLights.setup(lights, shadows, camera)
4. 重点是setup方法
```javascript
function setup( lights, shadows, camera ) {

		var r = 0, g = 0, b = 0;

		var directionalLength = 0;
		var pointLength = 0;
		var spotLength = 0;
		var rectAreaLength = 0;
		var hemiLength = 0;

		var viewMatrix = camera.matrixWorldInverse;

		for ( var i = 0, l = lights.length; i < l; i ++ ) {

			var light = lights[ i ];

			var color = light.color;
			var intensity = light.intensity;
			var distance = light.distance;

			var shadowMap = ( light.shadow && light.shadow.map ) ? light.shadow.map.texture : null;

			if ( light.isAmbientLight ) {

				r += color.r * intensity;
				g += color.g * intensity;
				b += color.b * intensity;

			} else if ( light.isDirectionalLight ) {

				var uniforms = cache.get( light );

				uniforms.color.copy( light.color ).multiplyScalar( light.intensity );
				uniforms.direction.setFromMatrixPosition( light.matrixWorld );
				vector3.setFromMatrixPosition( light.target.matrixWorld );
				uniforms.direction.sub( vector3 );
				uniforms.direction.transformDirection( viewMatrix );

				uniforms.shadow = light.castShadow;

				if ( light.castShadow ) {

					var shadow = light.shadow;

					uniforms.shadowBias = shadow.bias;
					uniforms.shadowRadius = shadow.radius;
					uniforms.shadowMapSize = shadow.mapSize;

				}

				state.directionalShadowMap[ directionalLength ] = shadowMap;
				state.directionalShadowMatrix[ directionalLength ] = light.shadow.matrix;
				state.directional[ directionalLength ] = uniforms;

				directionalLength ++;

			} else if ( light.isSpotLight ) {

				var uniforms = cache.get( light );

				uniforms.position.setFromMatrixPosition( light.matrixWorld );
				uniforms.position.applyMatrix4( viewMatrix );

				uniforms.color.copy( color ).multiplyScalar( intensity );
				uniforms.distance = distance;

				uniforms.direction.setFromMatrixPosition( light.matrixWorld );
				vector3.setFromMatrixPosition( light.target.matrixWorld );
				uniforms.direction.sub( vector3 );
				uniforms.direction.transformDirection( viewMatrix );

				uniforms.coneCos = Math.cos( light.angle );
				uniforms.penumbraCos = Math.cos( light.angle * ( 1 - light.penumbra ) );
				uniforms.decay = light.decay;

				uniforms.shadow = light.castShadow;

				if ( light.castShadow ) {

					var shadow = light.shadow;

					uniforms.shadowBias = shadow.bias;
					uniforms.shadowRadius = shadow.radius;
					uniforms.shadowMapSize = shadow.mapSize;

				}

				state.spotShadowMap[ spotLength ] = shadowMap;
				state.spotShadowMatrix[ spotLength ] = light.shadow.matrix;
				state.spot[ spotLength ] = uniforms;

				spotLength ++;

			} else if ( light.isRectAreaLight ) {

				var uniforms = cache.get( light );

				// (a) intensity is the total visible light emitted
				//uniforms.color.copy( color ).multiplyScalar( intensity / ( light.width * light.height * Math.PI ) );

				// (b) intensity is the brightness of the light
				uniforms.color.copy( color ).multiplyScalar( intensity );

				uniforms.position.setFromMatrixPosition( light.matrixWorld );
				uniforms.position.applyMatrix4( viewMatrix );

				// extract local rotation of light to derive width/height half vectors
				matrix42.identity();
				matrix4.copy( light.matrixWorld );
				matrix4.premultiply( viewMatrix );
				matrix42.extractRotation( matrix4 );

				uniforms.halfWidth.set( light.width * 0.5, 0.0, 0.0 );
				uniforms.halfHeight.set( 0.0, light.height * 0.5, 0.0 );

				uniforms.halfWidth.applyMatrix4( matrix42 );
				uniforms.halfHeight.applyMatrix4( matrix42 );

				// TODO (abelnation): RectAreaLight distance?
				// uniforms.distance = distance;

				state.rectArea[ rectAreaLength ] = uniforms;

				rectAreaLength ++;

			} else if ( light.isPointLight ) {

				var uniforms = cache.get( light );

				uniforms.position.setFromMatrixPosition( light.matrixWorld );
				uniforms.position.applyMatrix4( viewMatrix );

				uniforms.color.copy( light.color ).multiplyScalar( light.intensity );
				uniforms.distance = light.distance;
				uniforms.decay = light.decay;

				uniforms.shadow = light.castShadow;

				if ( light.castShadow ) {

					var shadow = light.shadow;

					uniforms.shadowBias = shadow.bias;
					uniforms.shadowRadius = shadow.radius;
					uniforms.shadowMapSize = shadow.mapSize;
					uniforms.shadowCameraNear = shadow.camera.near;
					uniforms.shadowCameraFar = shadow.camera.far;

				}

				state.pointShadowMap[ pointLength ] = shadowMap;
				state.pointShadowMatrix[ pointLength ] = light.shadow.matrix;
				state.point[ pointLength ] = uniforms;

				pointLength ++;

			} else if ( light.isHemisphereLight ) {

				var uniforms = cache.get( light );

				uniforms.direction.setFromMatrixPosition( light.matrixWorld );
				uniforms.direction.transformDirection( viewMatrix );
				uniforms.direction.normalize();

				uniforms.skyColor.copy( light.color ).multiplyScalar( intensity );
				uniforms.groundColor.copy( light.groundColor ).multiplyScalar( intensity );

				state.hemi[ hemiLength ] = uniforms;

				hemiLength ++;

			}

		}

		state.ambient[ 0 ] = r;
		state.ambient[ 1 ] = g;
		state.ambient[ 2 ] = b;

		state.directional.length = directionalLength;
		state.spot.length = spotLength;
		state.rectArea.length = rectAreaLength;
		state.point.length = pointLength;
		state.hemi.length = hemiLength;

		state.hash.stateID = state.id;
		state.hash.directionalLength = directionalLength;
		state.hash.pointLength = pointLength;
		state.hash.spotLength = spotLength;
		state.hash.rectAreaLength = rectAreaLength;
		state.hash.hemiLength = hemiLength;
		state.hash.shadowsLength = shadows.length;

	}
```
上面的代码是将灯光装换成uniforms数据，最终传递给GLSL。
5. 上面的流程中并没有将灯光进行传递，接下来。回到render方法，设置了灯光之后，设置渲染目标，确定背景，接下来调用renderObjects去渲染场景下的所有网格。
6. renderObjects通过循环调用renderObject
7. renderobject通过判断是否需要即时渲染而调用renderObjectImmediate，或者使用renderBufferDirect。
8. 最终 renderObjectImmediate和renderBufferDirect都会调用setProgram().
9. 在setProgram()中会判断是否需要initMaterial()；
10. 在getParameters的时候，会将lights全部设置到属性中
```javascript
var parameters = programCache.getParameters(
    material, 
    lights.state,
    shadowsArray,
    fog,
     _clipping.numPlanes,
    _clipping.numIntersection,
    object );
```

## 总结一下

1. 当render的时候，threejs会先初始化renderState
2. 为renderState添加sense中的lights，
3. 在renderObject时，将材质中的lights合并到renderState中。
4. 最后通过webGLPrograms.getParamesters(),将光照设置到Programs中。
