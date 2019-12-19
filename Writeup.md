# Camera Based 2D Feature Tracking

## The Data Buffer

### Task MP.1

* Solution: Lines 68 ~ 76 at MidTermProject_Camera_Student.cpp
```
if(dataBuffer.size() >= dataBufferSize)
{
    dataBuffer.erase(std::begin(dataBuffer));
    dataBuffer.push_back(frame);
}
else
{
    dataBuffer.push_back(frame);
}
```

## Keypoint Detection

### TASK MP.2

* Solution: Lines 91 ~ 112 at MidTermProject_Camera_Student.cpp
```
if (detectorType.compare("SHITOMASI") == 0)
{
    detKeypointsShiTomasi(keypoints, imgGray, false);
}else if(detectorType.compare("HARRIS") == 0)
{
    detKeypointsHarris(keypoints, imgGray, false);
}else if(detectorType.compare("FAST") == 0)
{
    detKeypointsFast(keypoints, imgGray, false);
}else if(detectorType.compare("BRISK") == 0)
{
    detKeypointsBrisk(keypoints, imgGray, false);
}else if(detectorType.compare("ORB") == 0)
{
    detKeypointsOrb(keypoints, imgGray, false);
}else if(detectorType.compare("AKAZE") == 0)
{
    detKeypointsAKAZE(keypoints, imgGray, false);
}else if(detectorType.compare("SIFT") == 0)
{
    detKeypointsSIFT(keypoints, imgGray, false);
}
```
* Solution: Lines 92 ~ 300 at matching2D_Student.cpp

### TASK MP.3

* Solution: Lines 119 ~ 136 at MidTermProject_Camera_Student.cpp
```
bool bFocusOnVehicle = true;
cv::Rect vehicleRect(535, 180, 180, 150);
if (bFocusOnVehicle)
{
    auto it=keypoints.begin();
    while(it != keypoints.end())
    {
        if(!vehicleRect.contains((*it).pt))
        {
            keypoints.erase(it);
        }
        else
        {
            it++;
        }

    }
}
```

## Descriptor Extraction & Matching

### TASK MP.4

* Solution: Lines 60 ~ 83 at matching2D_Student.cpp
```
if (descriptorType.compare("BRISK") == 0)
{

    int threshold = 30;        // FAST/AGAST detection threshold score.
    int octaves = 3;           // detection octaves (use 0 to do single scale)
    float patternScale = 1.0f; // apply this scale to the pattern used for sampling the neighbourhood of a keypoint.

    extractor = cv::BRISK::create(threshold, octaves, patternScale);
}else if(descriptorType.compare("BRIEF") == 0)
{
    extractor = cv::xfeatures2d::BriefDescriptorExtractor::create();
}else if(descriptorType.compare("ORB") == 0)
{
    extractor = cv::ORB::create();
}else if(descriptorType.compare("FREAK") == 0)
{
    extractor = cv::xfeatures2d::FREAK::create();
}else if(descriptorType.compare("AKAZE") == 0)
{
    extractor = cv::AKAZE::create();
}else if(descriptorType.compare("SIFT") == 0)
{
    extractor = cv::xfeatures2d::SIFT::create();
}
```

### TASK MP.5&MP.6

* Solution: Lines 20 ~ 28 at matching2D_Student.cpp
```
else if (matcherType.compare("MAT_FLANN") == 0)
{
    if (descSource.type() != CV_32F)
    {
        descSource.convertTo(descSource, CV_32F);
        descRef.convertTo(descRef, CV_32F);
    }
    matcher = cv::DescriptorMatcher::create(cv::DescriptorMatcher::FLANNBASED);
}
```
* Solution: Lines 36 ~ 51 at matching2D_Student.cpp
```
else if (selectorType.compare("SEL_KNN") == 0)
{ // k nearest neighbors (k=2)

    vector<vector<cv::DMatch>> knn_matches;
    matcher->knnMatch(descSource, descRef, knn_matches, 2); // finds the 2 best matches

    // filter matches using descriptor distance ratio test
    double minDescDistRatio = 0.8;
    for (auto it = knn_matches.begin(); it != knn_matches.end(); ++it)
    {
        if ((*it)[0].distance < minDescDistRatio * (*it)[1].distance)
        {
            matches.push_back((*it)[0]);
        }
    }
}
```

## Performance Evaluation

### TASK MP.7

* The number of keypoints of the preceding vehicle detected by the seven methods can be seen in the file MP7.keypoints_preceding.csv.

### TASK MP.8

* The number of matched keypoints for all 10 images using all possible combinations of detectors and descriptors can be seen in file MP8.matching_points.csv.

### TASK MP.9

* The top 3 detector / descriptor combinations:
    * FAST + BRIEF
    * FAST + BRISK
    * FAST + ORB
* The log time of all the combinations can be seen in the file MP9.log_time.csv.
