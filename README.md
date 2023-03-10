# image-slicer
Split images into tiles. 

Perform advanced image processing in Go - slice images into tiles, combine the tiles.

[![Go](https://github.com/goferHiro/image-slicer/actions/workflows/go.yml/badge.svg?branch=main)](https://github.com/goferHiro/image-slicer/actions/workflows/go.yml)
[![Go.Dev](https://img.shields.io/badge/go.dev-reference-007d9c?logo=go&logoColor=white)](https://pkg.go.dev/github.com/goferHiro/image-slicer?tab=doc)

## Idea and Inspiration

I was tasked with building a puzzle game in Go. I struggled to find an article or a library that can accomplish image slicing. 
Eventually, I  had to settle down with python's [image-slicer](https://pypi.org/project/image-slicer), and build a microservice 
[Game-Engine](https://github.com/pythoneerHiro/game-engine).

This incident made me realize that I can build a 35x faster library in Go. Hence, the inception of [image-slicer](https://github.com/goferHiro/image-slicer).

### Usecase 1: Split images into tiles

<details open>
    
<summary>Grid [2*2] </summary>

![hiro 2x2](https://user-images.githubusercontent.com/103487904/209455834-1886136b-3b6e-44f9-89af-66a83679b9b7.png)

</details>

<details open>
    
<summary>Grid [3*3] </summary>

![hiro3x3J](https://user-images.githubusercontent.com/103487904/209637746-18aa10da-1bd5-4996-99af-f4a217fdd410.png)

</details>

<details open>

<summary>Grid [4*4] </summary>

![hiro4*4](https://user-images.githubusercontent.com/103487904/209412028-9fa18329-bd99-4f55-9cd2-605794ac55b6.png)

</details>

<details open>

<summary>Grid [2*1] </summary>

![hiro2x1J](https://user-images.githubusercontent.com/103487904/210327504-e49498e6-29f5-416f-9094-93d8b3a82d7d.png)

</details>

<details open>

<summary>Grid [2*3] </summary>

![hiro2x3J](https://user-images.githubusercontent.com/103487904/210327651-de61b269-12dd-467a-8d45-41cf45d081eb.png)

</details>

<details open>

<summary>Grid [3*4] </summary>

![hiro3x4](https://user-images.githubusercontent.com/103487904/210327940-b029bd1f-5f68-42c3-a4f9-dbf5447a554a.png)

</details>

### Usecases 2: Join the tiles back together

<details open>

<summary>Grid [2*2] </summary>

![hiro 2x2R](https://user-images.githubusercontent.com/103487904/209455841-f3db61f6-49f5-45af-b32b-a26f9cfcbc65.png)

</details>

<details open>
    
<summary>Grid [3*3] </summary>

![hiro3x3R](https://user-images.githubusercontent.com/103487904/209637871-aa582b6c-7c3d-460a-9fc1-292942fdb2c2.png)

</details>

<details open>

<summary>Grid [4*4] </summary>

![hiro4*4R](https://user-images.githubusercontent.com/103487904/209412186-83ffec0c-acef-4d3b-b1b2-5c06c101078b.png)

</details>

<details open>

<summary>Grid [2*1] </summary>

![hiro2x1R](https://user-images.githubusercontent.com/103487904/210328209-ebf6abb3-48c0-4275-9a32-0bd937115293.png)

</details>

<details open>

<summary>Grid [2*3] </summary>

![hiro2x3R](https://user-images.githubusercontent.com/103487904/210328258-c4ecb5ed-9a61-478a-8967-5773a3e9fc9f.png)

</details>

<details open>

<summary>Grid [3*4] </summary>

![hiro3x4R ](https://user-images.githubusercontent.com/103487904/210328307-07d8a489-b540-4bb1-8dd5-e871074ee1dc.png)

</details>

## Support


You can file an [Issue](https://github.com/goferHiro/image-slicer/issues/new).
See documentation in [Go.Dev](https://pkg.go.dev/github.com/goferHiro/image-slicer?tab=doc)

## Getting Started

#### Download

```shell
go get -u github.com/goferHiro/image-slicer
```

# Example

### Slice Images

<details open>

<summary>Click to Expand</summary>

```go

imgUrl := "https://static.wikia.nocookie.net/big-hero-6-fanon/images/0/0f/Hiro.jpg/revision/latest?cb=20180511180437"

img := imageslicer.GetImageFromUrl(imgUrl)

if img == nil {
log.Fatalln("invalid image url or image format not supported!")
}

grid := [2]uint{2, 2} //rows,columns

tiles := imageslicer.Slice(img, grid)

expectedTiles := int(grid[0] * grid[1])

if len(tiles) != expectedTiles {
log.Fatalf("expected %d rcvd %d\n", expectedTiles, len(tiles))
}

```

### Join the tiles back 

<details open>

<summary>Click to Expand</summary>

```go
//lets join the tiles back

joinedImg, err := imageslicer.Join(tiles, grid)

if err != nil {
log.Fatalf("joining tiles failed due to %s", err)
}

shapeJ := joinedImg.Bounds()
shapeI := img.Bounds()

fmt.Println(shapeJ, shapeI) //shape might change due to pixel loss
}

```

### Get Images 

```go
//From base64

base64str := "" //very long.

img, err := imageslicer.GetImageFromBase64(base64str)


//From urls

imgUrl := "https://static.wikia.nocookie.net/big-hero-6-fanon/images/0/0f/Hiro.jpg/revision/latest?cb=20180511180437"

img := imageslicer.GetImageFromUrl(imgUrl)


```
[Inspired from ...](https://github.com/goferHiro/image-slicer/blob/main/imageslicer_test.go#L123)


</details>

## Important Notes/Caveats/Pitfalls

- Use the latest release.
- ```Slice(img,grid)```
    - ensure grid[0]*grid[1]>0.
    - ensure img is not nil.
    - its best if the img is downsized to the area of grid to avoid pixel losses. //Will bring support in future releases
- ```Join(tiles,grid)```
    - ensure that the grid is the same grid used originally for slicing.
    - the grid is utilized to figure out positions of tiles & the dimensions of the final image.
    - For best results, ensure the tiles were generated by [image-slicer](https://github.com/goferHiro/image-slicer).
- ```GetImageFromBase64(base64str) ```
    - Supports the image tag format i.e "data:image/jpeg;base64,/" as well
    - Supports jpeg,png only
    - Does not support svg, or any other formats
- ```GetImageFromUrl(imgUrl) ```
    - Expects jpeg,png formats only


## Contribution

---

- You can submit an issue or create a Pull Request (PR)
