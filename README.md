# vision-camera-face-detector

VisionCamera Frame Processor Plugin to detect faces using MLKit Vision Face Detector

## Installation

```sh
npm install vision-camera-face-detector
```

## Usage

```js
import * as React from 'react';
import { runOnJS } from 'react-native-reanimated';

import { StyleSheet } from 'react-native';
import {
  useCameraDevice,
  useFrameProcessor,
  runAtTargetFps,
} from 'react-native-vision-camera';

import { Camera } from 'react-native-vision-camera';
import { scanFaces, Face } from 'vision-camera-face-detector';

export default function App() {
  const [hasPermission, setHasPermission] = React.useState(false);
  const [faces, setFaces] = React.useState<Face[]>();

  const device = useCameraDevice('front');

  React.useEffect(() => {
    console.log(faces);
  }, [faces]);

  React.useEffect(() => {
    (async () => {
      const status = await Camera.requestCameraPermission();
      setHasPermission(status === 'granted');
    })();
  }, []);

  const frameProcessor = useFrameProcessor((frame) => {
    'worklet';
    runAtTargetFps(5, () => {
      const scannedFaces = scanFaces(frame);
      runOnJS(setFaces)(scannedFaces);
    });
  }, []);

  return device != null && hasPermission ? (
    <Camera
      style={StyleSheet.absoluteFill}
      device={device}
      isActive={true}
      frameProcessor={frameProcessor}
    />
  ) : null;
}

```

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT
