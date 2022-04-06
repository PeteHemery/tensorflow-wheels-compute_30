# tensorflow-wheels-compute_30
My old GPU has CUDA version 3.0. Most Tensorflow builds claim 3.5 is the minimum. Rebuilt Python wheels with 3.0 as the minimum.

Inspired by https://stackoverflow.com/questions/39023581/tensorflow-cuda-compute-capability-3-0-the-minimum-required-cuda-capability-is/50592978#50592978
and https://github.com/manojkumardas7/tensorflow_wheel_files

Deepspeech training requires Tensorflow version 1.15. More modern projects require Tensorflow 2.

Installed `bazelisk-linux-amd64` to handle downloading and running the correct version of bazel.

Need plenty of space and time to build this.

I symlinked my `~/.cache/bazel/_bazel_${USER}` folder to an external hard drive, since my local drive is nearly full.

(NOTE: bazel doesn't like spaces in path names)

Process for building:
```sh
git clone --depth 1 --branch r2.3 https://github.com/tensorflow/tensorflow
cd tensorflow
git apply ../0001-save-changes-to-build-with-compute-3.0.patch
TMP="/tmp"
./configure
time bazelisk-linux-amd64 build --local_ram_resources=14336 --config=opt --config=cuda --copt=-D"TF_EXTRA_CUDA_CAPABILITIES=3.0" //tensorflow/tools/pip_package:build_pip_package; alert; date
# Wait for 8 hours
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
ls /tmp/tensorflow_pkg/tensorflow*.whl
pip install /tmp/tensorflow_pkg/tensorflow-2.3.4-cp38-cp38-linux_x86_64.whl
```
