# Copyright 2020 The MediaPipe Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("//mediapipe/framework/tool:mediapipe_graph.bzl", "mediapipe_simple_subgraph")

# TODO: revert to private.
package(default_visibility = ["//visibility:public"])

licenses(["notice"])

exports_files([
    "hand_recrop.tflite",
])

mediapipe_simple_subgraph(
    name = "hand_landmarks_from_pose_gpu",
    graph = "hand_landmarks_from_pose_gpu.pbtxt",
    register_as = "HandLandmarksFromPoseGpu",
    deps = [
        ":hand_landmarks_from_pose_to_recrop_roi",
        ":hand_recrop_by_roi_gpu",
        ":hand_tracking",
        ":hand_visibility_from_hand_landmarks_from_pose",
        "//mediapipe/calculators/core:gate_calculator",
        "//mediapipe/calculators/image:image_properties_calculator",
        "//mediapipe/modules/hand_landmark:hand_landmark_gpu",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_landmarks_from_pose_cpu",
    graph = "hand_landmarks_from_pose_cpu.pbtxt",
    register_as = "HandLandmarksFromPoseCpu",
    deps = [
        ":hand_landmarks_from_pose_to_recrop_roi",
        ":hand_recrop_by_roi_cpu",
        ":hand_tracking",
        ":hand_visibility_from_hand_landmarks_from_pose",
        "//mediapipe/calculators/core:gate_calculator",
        "//mediapipe/calculators/image:image_properties_calculator",
        "//mediapipe/modules/hand_landmark:hand_landmark_cpu",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_landmarks_to_roi",
    graph = "hand_landmarks_to_roi.pbtxt",
    register_as = "HandLandmarksToRoi",
    deps = [
        "//mediapipe/calculators/core:split_landmarks_calculator",
        "//mediapipe/calculators/util:rect_transformation_calculator",
        "//mediapipe/modules/hand_landmark/calculators:hand_landmarks_to_rect_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_recrop_by_roi_gpu",
    graph = "hand_recrop_by_roi_gpu.pbtxt",
    register_as = "HandRecropByRoiGpu",
    deps = [
        "//mediapipe/calculators/image:image_properties_calculator",
        "//mediapipe/calculators/tensor:image_to_tensor_calculator",
        "//mediapipe/calculators/tensor:inference_calculator",
        "//mediapipe/calculators/tensor:tensors_to_landmarks_calculator",
        "//mediapipe/calculators/util:alignment_points_to_rects_calculator",
        "//mediapipe/calculators/util:landmark_letterbox_removal_calculator",
        "//mediapipe/calculators/util:landmark_projection_calculator",
        "//mediapipe/calculators/util:landmarks_to_detection_calculator",
        "//mediapipe/calculators/util:rect_transformation_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_recrop_by_roi_cpu",
    graph = "hand_recrop_by_roi_cpu.pbtxt",
    register_as = "HandRecropByRoiCpu",
    deps = [
        "//mediapipe/calculators/image:image_properties_calculator",
        "//mediapipe/calculators/tensor:image_to_tensor_calculator",
        "//mediapipe/calculators/tensor:inference_calculator",
        "//mediapipe/calculators/tensor:tensors_to_landmarks_calculator",
        "//mediapipe/calculators/util:alignment_points_to_rects_calculator",
        "//mediapipe/calculators/util:landmark_letterbox_removal_calculator",
        "//mediapipe/calculators/util:landmark_projection_calculator",
        "//mediapipe/calculators/util:landmarks_to_detection_calculator",
        "//mediapipe/calculators/util:rect_transformation_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_tracking",
    graph = "hand_tracking.pbtxt",
    register_as = "HandTracking",
    deps = [
        ":hand_landmarks_to_roi",
        "//mediapipe/calculators/core:previous_loopback_calculator",
        "//mediapipe/modules/slr_landmark/calculators:roi_tracking_calculator",
    ],
)

# TODO: parametrize slr_landmark graph with visibility and make private.
mediapipe_simple_subgraph(
    name = "hand_wrist_for_pose",
    graph = "hand_wrist_for_pose.pbtxt",
    register_as = "HandWristForPose",
    visibility = ["//visibility:public"],
    deps = [
        "//mediapipe/calculators/core:constant_side_packet_calculator",
        "//mediapipe/calculators/core:side_packet_to_stream_calculator",
        "//mediapipe/calculators/core:split_landmarks_calculator",
        "//mediapipe/calculators/util:set_landmark_visibility_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_landmarks_left_and_right_gpu",
    graph = "hand_landmarks_left_and_right_gpu.pbtxt",
    register_as = "HandLandmarksLeftAndRightGpu",
    deps = [
        ":hand_landmarks_from_pose_gpu",
        "//mediapipe/calculators/core:split_landmarks_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_landmarks_left_and_right_cpu",
    graph = "hand_landmarks_left_and_right_cpu.pbtxt",
    register_as = "HandLandmarksLeftAndRightCpu",
    deps = [
        ":hand_landmarks_from_pose_cpu",
        "//mediapipe/calculators/core:split_landmarks_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_landmarks_from_pose_to_recrop_roi",
    graph = "hand_landmarks_from_pose_to_recrop_roi.pbtxt",
    register_as = "HandLandmarksFromPoseToRecropRoi",
    deps = [
        "//mediapipe/calculators/util:landmarks_to_detection_calculator",
        "//mediapipe/calculators/util:rect_transformation_calculator",
        "//mediapipe/modules/slr_landmark/calculators:hand_detections_from_pose_to_rects_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "hand_visibility_from_hand_landmarks_from_pose",
    graph = "hand_visibility_from_hand_landmarks_from_pose.pbtxt",
    register_as = "HandVisibilityFromHandLandmarksFromPose",
    deps = [
        "//mediapipe/calculators/core:split_landmarks_calculator",
        "//mediapipe/calculators/util:landmark_visibility_calculator",
        "//mediapipe/calculators/util:thresholding_calculator",
    ],
)

mediapipe_simple_subgraph(
    name = "slr_landmark_gpu",
    graph = "slr_landmark_gpu.pbtxt",
    register_as = "SlrLandmarkGpu",
    visibility = ["//visibility:public"],
    deps = [
        ":hand_landmarks_left_and_right_gpu",
        "//mediapipe/calculators/core:flow_limiter_calculator",
        "//mediapipe/calculators/image:image_transformation_calculator",
        "//mediapipe/modules/pose_landmark:pose_landmark_gpu",
    ],
)

mediapipe_simple_subgraph(
    name = "slr_landmark_cpu",
    graph = "slr_landmark_cpu.pbtxt",
    register_as = "SlrLandmarkCpu",
    visibility = ["//visibility:public"],
    deps = [
        ":hand_landmarks_left_and_right_cpu",
        "//mediapipe/calculators/core:flow_limiter_calculator",
        "//mediapipe/calculators/image:image_transformation_calculator",
        "//mediapipe/modules/pose_landmark:pose_landmark_cpu",
    ],
)
