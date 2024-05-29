# Image-Slider-Carousel-Project

![Image-Slider-Carousel-Project](Image-Slider.gif)

Image Slider Carousel Project

Images might come from dummy data or an API 

like this link for example

https://picsum.photos/v2/list?page=1&limit=10

data will look like this
[{"id":"0","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/yC-Yzbqy7PY","download_url":"https://picsum.photos/id/0/5000/3333"},{"id":"1","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/LNRyGwIJr5c","download_url":"https://picsum.photos/id/1/5000/3333"},{"id":"2","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/N7XodRrbzS0","download_url":"https://picsum.photos/id/2/5000/3333"},{"id":"3","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/Dl6jeyfihLk","download_url":"https://picsum.photos/id/3/5000/3333"},{"id":"4","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/y83Je1OC6Wc","download_url":"https://picsum.photos/id/4/5000/3333"},{"id":"5","author":"Alejandro Escamilla","width":5000,"height":3334,"url":"https://unsplash.com/photos/LF8gK8-HGSg","download_url":"https://picsum.photos/id/5/5000/3334"},{"id":"6","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/tAKXap853rY","download_url":"https://picsum.photos/id/6/5000/3333"},{"id":"7","author":"Alejandro Escamilla","width":4728,"height":3168,"url":"https://unsplash.com/photos/BbQLHCpVUqA","download_url":"https://picsum.photos/id/7/4728/3168"},{"id":"8","author":"Alejandro Escamilla","width":5000,"height":3333,"url":"https://unsplash.com/photos/xII7efH1G6o","download_url":"https://picsum.photos/id/8/5000/3333"},{"id":"9","author":"Alejandro Escamilla","width":5000,"height":3269,"url":"https://unsplash.com/photos/ABDTiLqDhJA","download_url":"https://picsum.photos/id/9/5000/3269"}]


- if it’s dummy data we take this dummy data and pass it as a props to our component to be rendered as a slider

but if it’s an API link we first needs to fetch image urls by receiving this API Link as a props then create the slider 

so for this project

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

