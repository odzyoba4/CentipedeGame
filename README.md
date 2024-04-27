# CentipedeGame
Video Demo: <br>
[![CentipedeDemo](https://img.youtube.com/vi/yPkErqfitr4/0.jpg)](https://www.youtube.com/watch?v=yPkErqfitr4) <br>
## Overview
This project is an attempted replica of the Atari Centipede game. During this project I learned how to handle systems that totaled up to 100 classes, and how to use design patterns like factory pattern, command pattern, strategy pattern, finite state machine, and others. <br/>
Game Controller Class Diagram:
![CentipedeGameFlow](/images/CentipedeFSM.png)

## Finite State Machine
One of the systems I implemented was the main centipede's movement. To do this, I used a finite state machine to create static state classes to decide between 
## Mirror
One of the features I implemented is a mirror effect, where the illusion of reflection is given by rendering objects from one side of the mirror twice. <br/>
Youtube Demo: <br/>
[![MirrorDemo](https://img.youtube.com/vi/eR4eGSRtDbU/0.jpg)](https://www.youtube.com/watch?v=eR4eGSRtDbU) <br>
While costly when used for a real scene, this effect got me to be familiar with using the stencil buffer for multiple renders. To do this effect, I created a special mirror class that would handle different rasterizer states, blend states, and depth stencil states.
Here is a function that a user would call to render the mirror object into a stencil buffer to use later:
```C++
void Mirror::WriteMirrorToStencil(ID3D11DeviceContext* contextPtr, const Camera& cam) {
	//*//Render mirror without writing to render target and write to stencil buffer

	// BLEND STATE: Stop writing to the render target 
	contextPtr->OMSetBlendState(NoWriteToRenderTargetBS, nullptr, 0xffffffff);
	// STENCIL: Set up the stencil for marking ('1' for all pixels that passed the depth test. See comment at line 35)
	contextPtr->OMSetDepthStencilState(MarkMirrorDSS, 1);

	// Set Color Shader to context
	pShader->SetToContext(contextPtr);
	pShader->SendCamMatrices(cam.getViewMatrix(), cam.getProjMatrix());
	// Render the mirror object
	pShader->SendWorldColor(worldMat, Colors::DarkGray); // The color is irrelevant here
	pPlane->Render(contextPtr);

	// STENCIL: stop using the stencil
	contextPtr->OMSetDepthStencilState(0, 0);
	// BLEND STATE: Return the blend state to normal (writing to render target)
	contextPtr->OMSetBlendState(0, nullptr, 0xffffffff);


	//////Prepare Rasterizer and depth stencil state for rendering "reflected" objects
	
	// WINDINGS: face winding will appear inside out after reflection. Switching to CW front facing
	contextPtr->RSSetState(MirrorFrontFaceAsClockWiseRS);
	// STENCIL: Use the stencil test (reference value 1) and only pass the test if the stencil already had a one present
	contextPtr->OMSetDepthStencilState(DrawReflectionDSS, 1);
}
```

