img = Image.open(r"D:\Location-of-all-sites-on-Islamabad-map.png")
img_arry = np.array(img)

da = xr.DataArray(
    img_arry, dims=['y', 'x', 'band'] if img_arry.ndim==3 else('y', 'x')
)

minX, minY, maxX, maxY = [72.8, 33.6, 73.2, 33.9]
height, width = da.shape[0], da.shape[1]

x = np.linspace(minX, maxX, width)
y = np.linspace(maxY, minY, height)

da = da.assign_coords({'x':x, 'y':y})
da.rio.write_crs("EPSG:4326", inplace=True)
da_transposed = da.transpose("band", 'y', 'x')
da_transposed.rio.to_raster("Islamabad georeferenced.tif")
