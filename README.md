# Image-Slider-Carousel-Project

![Image-Slider-Carousel-Project](Image-Slider.gif)

Image Slider Carousel Project

Images might come from dummy data or an API 

Props:
The component accepts three props:
url: The base URL for fetching images.
limit: The number of images to fetch per page (default is 10).
page: The page number to fetch images from (default is 1).


State Variables:
images: Stores the fetched images.
currentSlide: Tracks the index of the currently displayed slide.
errorMsg: Stores any error messages that occur during fetching.
loading: Tracks whether the images are currently being fetched.



Fetch Images Function
  async function fetchImages(getUrl) {
    try {
      setLoading(true);

      const response = await fetch(
        `${getUrl}?https://picsum.photos/v2/list?page=${page}&limit=${limit}`
      );
      const data = await response.json();

      if (data) {
        setImages(data);
        setLoading(false);
      }
    } catch (e) {
      setErrorMsg(e.message);
      setLoading(false);
    }
  }

fetchImages is Asynchronous function that fetches images from the provided URL.
setLoading(true): Sets the loading state to true before fetching.
Fetch Request: Sends a request to the provided URL with the page and limit query parameters.
Response Handling: If the data is received successfully, it updates the images state and sets loading to false.
Error Handling: If an error occurs, it updates the errorMsg state and sets loading to false.

Navigation Functions
  function handlePrev() {
    setCurrentSlide(currentSlide === 0 ? images.length - 1 : currentSlide - 1);
  }
  function handleNext() {
    setCurrentSlide(currentSlide === images.length - 1 ? 0 : currentSlide + 1);
  }
handlePrev: Sets the currentSlide state to the previous slide, looping back to the last slide if currently at the first slide.
handleNext: Sets the currentSlide state to the next slide, looping back to the first slide if currently at the last slide.



useEffect Hook

  useEffect(() => {
    if (url !== "") fetchImages(url);
  }, [url]);

useEffect: Calls the fetchImages function whenever the url prop changes.

Conditional Rendering
//  console.log(images);
  if (loading) {
    return <div>Loading...</div>;
  }
  if (errorMsg !== null) {
    return <div>Error: {errorMsg}</div>;
  }
Loading State: Displays "Loading..." when loading is true.
Error State: Displays the error message if errorMsg is not null.


Rendering the Image Slider

  return (
    <div className="container">
      <BsArrowLeftCircleFill
        onClick={handlePrev}
        className="arrow arrow-left"
      />
      {images && images.length
        ? images.map((imageItem, index) => (
            <img
              className={
                currentSlide === index
                  ? "current-slide"
                  : "current-slide hide-current-slide"
              }
              key={imageItem.id}
              alt={imageItem.download_url}
              src={imageItem.download_url}
            />
          ))
        : null}
      <BsArrowRightCircleFill
        onClick={handleNext}
        className="arrow arrow-right"
      />
      <span className="circle-indicators">
        {images && images.length
          ? images.map((_, index) => (
              <button
                key={index}
                className={
                  currentSlide === index
                    ? "current-indicator"
                    : "current-indicator inactive-indicator"
                }
                onClick={() => setCurrentSlide(index)}
              ></button>
            ))
          : null}
      </span>
    </div>
  );
}

Container Div: Contains the entire slider component.
Left Arrow: Clickable icon to navigate to the previous slide.
Images:
Maps over the images array and displays each image.
Applies the current-slide class to the current slide and hide-current-slide class to the other slides.
Right Arrow: Clickable icon to navigate to the next slide.
Circle Indicators:
Displays a button for each image.
Applies the current-indicator class to the current slide's indicator and inactive-indicator to the others.
Clickable to directly navigate to the respective slide.

