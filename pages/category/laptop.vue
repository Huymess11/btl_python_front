<script setup>
import CategoryProducts from "~/components/common/CategoryProducts.vue";
import { ref } from 'vue'
import {ShoppingCart} from "@element-plus/icons-vue";
import {fakeProducts} from "@/constants/fakeData.js"
import spaFetch from "~/plugins/fetch.js";


const { $apiUrl } = useNuxtApp()

const products = ref(fakeProducts)
const router = useRouter()
const value = ref('Option1')
const loading = ref(false)

const formState = reactive({
    page: 1,
    size: 20,
    total: 0
})

const getProducts = () =>{
    loading.value = true
    spaFetch(false)($apiUrl.PRODUCT,{
        method: 'GET',
        params:{
            page: formState.page,
            pageSize: formState.size,
            search: 'laptop',
        },

    }).then( res => {
        products.value = res.results.map(item =>{
            return {
                ...item,
                status: item.type
            }
        })
        loading.value = false
        formState.total = res.count
    }).catch( error => {
        loading.value = false
        console.log("error", error.response)
    })
}

const advertising =
    {
        img1:'data:image/jpeg;base64,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',
        img2:'data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAoHCBUUFBgUFBUZGBgYGBgbGhgbGhsZGxsYGRgaGRgbGhobIC0kGx0pHhkaJTcmKS4wNDQ0GiM5PzkxPi0yNDABCwsLEA8QHhISHTIpIysyMjQyMjIwMDIyMjAyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMDIyMjIyMDIyMjIyMjIyMv/AABEIAIUBewMBIgACEQEDEQH/xAAbAAABBQEBAAAAAAAAAAAAAAAFAAEDBAYCB//EAEYQAAIBAgMEBgYHBAkEAwAAAAECEQADBBIhBTFBUQYTImFxgQcyQpGhsRRScrLB0fBic4LhFSMkM5Kis8LxJTRD0zVTVP/EABkBAQEBAQEBAAAAAAAAAAAAAAABAgMEBf/EACkRAAICAgIBAwQBBQAAAAAAAAABAhEDIRIxQQRRcRMiYYGhQpGxwfH/2gAMAwEAAhEDEQA/APKXWnBrpjTAVDod2xRfC2wBmbQCNaH4W1JoxiVi2BzHz/lXOcuKNQjZBfucB+uQoFirkseQn+VE7j5VJ5An8qEDQLOsnXyMfnUxiR6B0X2K2IuJYHsWHuOZ9t8qCe+Wb/BVJ9itndGhSji20/XJIHkAC3hHOtl0HssmFuYuIbE3JTus2iVQebZjWY6ZbdZrzZUVS2pM+2VCZyPaIURG7WjvwaTXnos+kazbOHwAsf3aJcRf8hk95gn31541k1qcHjHxISy6jKpkZFKgcCWJJ5mi+PwGzsOgBRr90iYZnS2o3S0GYnQDeaxCTiuL29hxT2ujCW9nXGXMEbKdxiAfAnQ1f2NssNdUXAAo3hmCqx5FidF4nwjjVu9iXaSePy4DwqJJNbcm1SMVR6p/SdvCYR3tZWAss63FjK7KhKgR6oBiFrwe4SxJJkkkk8ydSa0rLE7td/f486oX8NbbcIPMaD3VnH9hZPkDLGFd/VUn5e86VYTZlzMAy5Qd5JBAHPQ0WsWIAHADT9c66xCbhHie+kszsKCNFsPZFu2gYgEnduJ8WI4/s7hWV6T43rMQwHqISi+IPaPmfkKmVCuo0I5afEVDesq2pGvMfjzrMMi8mpxvoGKtW0SFk8aSWwO+uW767XZjoZqUUwroVsg6wCMwMTrEAxxgkEA+VFsTgLQuOls3AEw5uyxViW6tHC9lVhe3HPShJ3UfTFZL924jgEYSEYEev9HtgAHi0giPGgK30C0mdrjOUS1ZudgorE3er0zMrAAZzw4CquzcKLt5LRJVXdVkQSFZo0nSYo4MRduXMQbbzcfDYYTnQFj/AFOcSxAmAZHdQrYJC4mzmIAF1JJIAEMN53R30BJhcJYu3bFu3cdVuMRczFXe3DsCZVVB7AzRFRnZxFrEXJOaxdS3lj1gzOjHxDKg/jooEyi0L7hWTC4kSCrsuZnt2xCt2jDggTuHdVu3j7Nu7edmDW3v2nMCSQbFy6CFBMf1oQHluoaBuP2basNeW4zvkayi5Siy1yw1xi2ZTIBEACDQV3gE8hWjxT3LiYgW2BznCFwHUZkGFYP6xGYB4mNZoFhsE15XCEZlSQpIUvLKpALEAEBs3gDQBQbIXrDbztAxYw8wJykOc/2uzu3a1Vt4e19HS4zOtx0uOmq5M6PkFuMuYkrrOYRI8zeHXPfuMpUhNoLcY50ACAXAXEkSvhNCNn4U/Q2ZipttbNwAlQ1u9bcIoAJmXSQYGoK/VFCWQ461aW3aa31kurMc7IQAr3LcDKoO9Jk8476olqs4twbeHAMkW3BHI/Sb5g8tCD5iqRNCnc01cE04agHao2Ndk1E5oBTXDGmY1yWoZHJqN6RauDQHJpqc0xoCzgsJ1hPaywBwnf50QtbAZ/VZ21A7KFtTuGh7qh2KdW8F+Z316bs/bmCt21hVVytvMotz2lPbzMD282sboG6vZjxRlC62fO9R6icJ1F6POsZ0buWkLXM6aMQHtlJyiSBJoDNeodL9qJiFPVMzKiXyCwj1wIAHCAvhXl1edxdLkq/6dsGVzT2Xi81PYSTUJtkRPEA+R3fn5iiWBsTXI9tbDdnCpbwhuMNcytMahAwUx5Zj31wDbvoHtsFGohtN2k901bxGHL2zbX1Qh+6YFVNg2jasK24sJkqG0bWBm3aV4ZScouT99fB7Fh4yr8b+QHtq2UAU+0eBB0GvDvim2Nsd8VctWk3uVQd0klmPcok+VdbXBu3gttTMBYAElj2mMDuE16b6P9lrZttfZe0qlE03kxnI58F82rq8qhGKfbOP0+TbrS1+zUY027VtbNvRLaBEHJVECvK9tYF7l4wJ7+VarGY5nc68d38qbqQsrx3k8zXOeetnSGBdMoYHCLZt+AJY+A1NYJrzXr5uMTDPIXwEL8K9M2yotYC/caJKhF+05CfIk+Ved7KxVu3dR3tm4FMsgbJIgwC8GNY4VvA3K5HPPJJqKCAtE8NI1/nVR7oGi69/631d6Q7fOJbs20sWx6ttPgXeAXb4DlxoSskgASSQABqSToABxPCK38nB7OiSx51KuGOZR3ifDeR8K2+F9Hl5LAu3CA5WSnFAfZJG9ucbt1ZzE4W4rdnLIPESOXA6UW3TL40RC1FRlBrJ8qbDYu66sxtFlVmUsmuq79DrU1rE4drV09Y/X6Lbt5WAGb17jkiIUTAneRWpY0FJg7EYlFIXex4a/GK5yO0MwLIGgwcoBgkDTmATx3VGmF/MniTzmu7W8qCWI/HfHw0qcUuhybOHhdABVV2mrN2oCnlWkUipKaRFMK6GCzhEDXEU7mdQfAkCvVNrdFNj4S4lm/fuo7hSq5S0hmKjVLZA1B315Zs/+9t/bT7wr0v0qYS5c2hYFlGdxZRgqgsezcczA1jdVIZ/p70atYPFW7NrMUdEY5oJlnZTuA4CtTtfopsjBulvEX7yM6gqMpaQTlGqWyBqKyXSzH4q/ibb4y0LThUUKFZZUOSDDGd5InurQ+l4/wBtw/7tf9RqAHdLOjeEweMsWmd1sOqtcfeyqWYEgKuu4cDvrQYXovsi5h3xSX7ptWzDvlIg6eybeY+sNwob6Zz/AGq1+5H33rvo0f8AoOM+3/6qDwY7pPbwi3YwTs9rKO0ylTnk5hDKpgacKKejzo5ax1+4l4sFRM4yxM5gNZB01rJ16N6Fx/ar37n/AHrQ0+i/snonsjGM9vD3rrui5mGUrAmBq9sA68qyXQLo/axuLa1dzBRbZ5WAZBUAag6do1pfRAP7Viv3Q+/VL0Pn/qFz9w/37dDPucbWwexES4tvE3TdRXCoUcAuAYBbqgIkRM1k+jWzlxOLs2GJC3HykjeFALGO+FNS9JNj37Vx7ly06I1x8rMpUGSSIJHKjfokwufaSNwtpcfzK9WPv0L4KnpB6PW8DiRbtlijIrAtBMkkHcBy+NTdFejlrEYHF4lywaxOUAjKYth+1IneeFHPS8wvW8Hil3XLb7v4GH3jUXo+/wDiNpfxf6IoTweaM1RM9Pcao2ahRy1cE1yTTE0A2akTTGlQDzTTSrmaA03QvZt2+7i1AdAjAn7UDgeNb76LjbS9vCO8CC6XA06AE5UUkceHHuFY70a9ILGDvXWxLMqugAYKW7QYGCBrqPlXoL9PdlFixvtw06l+AA4DurvjyUqfR5M2Jt2uzIbWxZe3dRwyZbbwjAyCFI1LAH315wa9r6TdP9n3MFcsWnZ3ayyKDbb1iuUHMw03zXik0zZFkqlRfT4XiTt3YRwyya0dx7dsT6oySO8qQrePrAx41n8GNan2qpKcypB9+/515Jbkv2fSg0oN1vVG02Pcz285gdjNv3DKTUu18Ili2C7BYQGI13kAd5iBVv0dbN+kC0DGUKGYGPVUxHfLAA9wPdQ/0xuvX21DSVlTG4kQzE84zKPENyrh9K9XWzpLM+/xZmejmEa9fZ1HGB3TvOnIAe+vXcYxt20tq0ZEgsQCeExPE+def9AhcRRl3euw+tPqieW4+VaDG3Ll0hRvJka8QRy3H86+d6rI3la1SPVgwtwV/P8Acs4VJl37RLAKSoJnKd8RpyrrC4U3LhiCJ3gz/wAUdwGyC1sKYUgHUGYkQd9RY6wmCsXXsiWRDlJ0zXD2UnuDEVnAp5N1oxlywjddnnHpH23nujCWz2LB7RHG6RqB9kaeJNY/CNB3Hf8AOvQdneibGXe1dvWUJ1JzNcYkmSTEDXXjVja/opexbD28QLjTqpTIBzgljNfZio44V4PmNuUjBPR7oFZL4+0ogZs4UkbmCEgjkRBjxobiMMAxt+0phtQYPIxxohs21cssl2y2W4hzK0AwYI1B0IIJEd9cXKP9XTPTixSnqPZ6nt97+HtnM+ZToNa8xx+N9ZuIBjx4fGK1G1OkmNv4ZestI6sqvnSVPaWRKNI48xWFxGZ2AysvalpEaDXw3xWI1Gbp2vk19OTSTWzXdDLYTBtIn+tf7luu+jGxrGIuXrl5wpzKijmNWY/FR5VW2DejDNyNx/hlH4VoeiPRy3iMJ1jMVdrtzUaiFIUAjympFzlJ0d88YwhTLe1+heFW0XRxIHMa/GvJ9q4A2bhZDuNbfpLs+7hjlLEqdx4EViMfiyJ5nQePOu8XK+jwOKrsexft3ey3ZbmONQ43DIn/AJIHIjX3DfVMWSAInUiOeao3wzTLHXiSZNdFHeiO0tjM44e8/lTU62+6kVroZLGzv7239tPvCvU/SPtG7h9qYd7LlHNq2hYBT2WuuGEMCNRXl2zv71Ptp94V7D6QOi+JxWMs3rNvMiLbDHMo1W4zEQSDuIoRgH0vn+32P3Vv/VuVJ6X/APvcP+7T/UaofTCf7fY/dW/9W5Wg9InRfE4vFWrlm3mVEUE5lGodiRqRwIoAB6af+7tfuR996l6Nf/A4z7f/AKqg9NbRi7X7kffepejT/wDQcaf2/wD1UHg83zV6P6Fz/ar37n/eteZjfXpfoV/7u9+5/wB60LLo0XRzHYXC7N+npYJfW3eyHtFhcKZiHaIJytp9asn6GnnHvP8A+d/v26I+jK6L9vHYBzo650nmQUf3EWz50P8AQ8hXaVxSIIsOCORDpI99Ce5m+lXSXF4l3tX7xe3buuVQrbUAgso1VQToSNTWp9Eg6tMdieFuwAD3nrGP3FoTtroFjVa9fa1CA3HJzJoslpjNO6tH0ExgwOx8TjCiuTeACMYDQUthSYPFm4UHgqbXQ3Oj2Gc+tZuMh8A9xB/tpvR+f+kbT8G/0RRg7f8A6V2PjGNpLTWmByIcwhMlzNuGp7Q8qG+jLCvd2ZtC1bGZ3Yqo0Ek2gBqdKE8HlDGuGrUbb6EYzCWjevW8iAgE5kOrGF0DE76yxoaOaY0jSNAMaanNNQDU1O1NRAalSpqEFSpUqEDWzFlgP1uommF6wuo4giY7t9UtgsOtTun7pj41py5XOF9d4tqd5lyFBHhmJ05GvLkk1KkeuCXDfuEuje1xgsEMyzcfK1qCAwZiQsH+LwMkGRNY3b+AxebrsXbdWuCVZhAYE7l578x46zxqz0nXq8SFGqAIY7lOhyzqYBqTamPbE5ASzMi5RJZzAGpAJiAF91W3GvyKU061Va9wxsFOrRUU9p+0AupAAhQa9A2LstLaG5fyqcpLMSFCjmTwNZbBbVweCRYnEYkqAttICqQJ7b7ljkJI5UCx/SC7inPXwyDVUWQixrontH9ppPhur5U/T5Mk+Xjv5/R7J5/t4R0lqzZ7V9INpQbeCt9YRoXaVQd4Ghfx0HjWYfa+KxMi7iOwSpNtQFQgGSpCjUacSaE37ounJmClCITMBwPEgj2dx/44TOGYBXbvCMR4dkHUcpr6mOL40fOnSlo29vpPctjeCAdAJMSYCz8aGdIel142yqnKX0BB1A4n3fhQAtcJ1VlABYs4yhVG9mzbgP1rWdx+0s7ypJA0BOnnHCdKPEn2ahJl/YlqbpXmA3xg+PCtlfwhtpOXzrC7JxpS6HgGAR7yPyrX4zpMzpkUBARBXshT367j4V5fUqTkqPq+gxtPkqoNbGvK2CQSCwVlyyJ7LsN3gKz+2zlB0iqWA6SvatC3kzLLGSJBzMTx040P2ptxrqmJgToSunhxqY8UuTbXk9DlBRe1/s0mwbf9jUn2sze9mIqrsrpHisA7dXluWmaWtPunmjDVD7x3VzsvaFsWVQXIIVQVO4mNe8azqPcaGbXxKiSDwOlerHak2eHOlKCT8G4x3S/C42xluK9hyJXrBKTuIFxdP8WWvN79vNcY71UkA8DB1I7ifhFaLCKvVIpg5UUeeUT8ZoTi7gG6vRdngjjUX2U1btjuBP4D5/CosQ81yj+seZjyH/NQXXokZnKybKy61Xd5NJr5OhqOa2czpXIMgxGs8o1mtCdrbSDZeuxMghY6xvWbLAnNHtr7xWbMRFF7fSC4pJCrBuNcI1HaZSoG/QCRH2BQEV6/iMQxd2uXWTKhdiXKyTkXNPEzHnV65tnaKqXOIxARSVLdYxUEHKRmBjfpQ/Zm17lhCiKpDOjtmmSUZWQSDoJH+Y01/a9x8P8ARiAE6xrhInMWJbeeI7X+UUBNjVxd3t3utuZULZnJaEUZiQWO4Zp86azexK27tpWuC2mt1AxCiDEuswTK/wCXuqW/0juXCTcRG7CWwIYKqI5cALMGdBJ17IPOqlnaZUXAbaP1jq7FgTqrl4A5HMRzgmgJP6PvCR1TiDlPZOjdnQ9/bX/EKmwhxVnNcsm5bOTMzISh6uA0kgiViDXb9J7xUrCAM5uEAH18pWd+7cY5qKgxG3blxGRkSDbS2DBlUQuQRJ1MPE8lHKg2RYG9ftsLllriMUdg6EqciTnMj2RkM/Zq1ZXGWrjXENxLjRmdXyuesLESQZOYqx/hM7qrWdplVyi2mtk2STM5SzMxGujEvr4DnUp27cL52VXJcsQZA9VkVBlOiqGYjvM0BefGbSuAo1zEurl0Kl2IOWQ6kFu4jXfuFDbD4q5Z6q2br2M3qAnq85IYdndMkHxrsbbue0qsMtxQuuWbrMzuQD2icxEGRFNgduXLNo2UVChuLcJIJJZcuWTO4FAR586Aisvirdt1ttcRLiZnCtCukMJcA6iFffyNd7PxGNtW81i5dto+ZiUfIpyCGYwRu3a1CNr3Aj2wFC3Etod8hbYgRrx7RP2jTjbDdWLYtoItNbzCcxVnVySZ5pu3dpudAW8XiNoXkNu7cvugZAytcLDM0FAQTv7S+EiYoedlXuNphqy8IlAWbWeAU67tDVxOkNwEtlQsbpuSZOsABYmIGVf8NcNtpiD/AFaZit1M0tmi6xYnUxpJHfOtCbKWG2dduANbQsCwQER65E5dTy+dK3sy80ZbbmUziBMp9Ycx39x5VPY2vcS2ttQsKzON8lmVkJOusAiPsiuL+17hKlW6vLbFsBCR2Bm0POcxnnNAQ4bZ124oZLbMpbKCIjNGaNe4b91NZ2fdfMVtscrKrdzMwVV8SxA86s7P2wbIULbUwXJkt2ywhcwB9kbh3nnTYDa7WhGRXPWrczNmnMu6IMAySZ36mhNlc7MvRmyMRLCRB1SQ248IOvdXF/AXUUu6EKr5CdIDxOXQ74q9a2/cVUTKpVAQBrqGdHbNrqTlInkxqDHbSN1MpRR/WPcLSSSzEzvOm+NN+UcqDYNpU1KhbEaVNSqmQxsq4BcQnn/Kjz4yLwKqWK6qNIzssSzbhCn/ADd1ZbC5s6hQc2ZQBzaYA7ta0r2wrQCTvljpJLHXvHhXnyKmeqDuJXx9zrLmc6u0Lr6qgbgNSeZ/Kr+1bq4a0LVszdupDtoCqE9pdNxfd9kHnQ5DDF2HZSNPrMfVUeJ1PIAmiPRXYtzEu94xFvtFm9XNBMnwjQeHAVnjbTb0v5M8qTS7f+Cnh8KbawdGI1A9lfq+J41NgMK1+71RuC0mmdzrC79AN55D+dR3sWqsVnNqZbfr+NdI4Dlk4hQe9lkSO6IHlW+L7Mc9UbTpKuzMPg0sWree4pJVyxDyYLs7LEgkL2d2ggCKzOA2jdKZFfKiCSSSFUEySe8+8mhd3M7STpz/AC513iLZKBGuLbTeFJ1b9puM1ZN0IRtkvSLEq6pbtuchGZ53uwMIX7t5A4Tzms6LHMj51a2ittMotXC+naOUrlbkJ9Yd9Ds9agtFlJJtBnZ/VL69wr/AW+7V18RZ3C5P8DAec6/CsznpdYazLCpO7Z1x+qlBUjTN1YGlxCCd2bVW0AMHXKdAdNNDwNRsgmGUSDqCBvrPC6aM7NxPWLkPrqNP2kA3d7KPeo7tdQjSqzjknylySr4LH0O2fZjwJFJtlq3tt4GGqVDVi21b4owsk15IhgrnssvxE+NVsVs6+d1ot9khvkZo3YNFMIJIA40pF+ozA3LToAHR1ge0rLrx3iqrMDxr2BEZdGBE6iRoRvG+ob+ysPc9ezbbvKAH3jWlE5nkRNMTW/210RsshNhcjjUDMSjfskEnL415/cQqSrAhlJBB3gjeDQXYga4usQdCYOtIV0wlfDX8/wAKISIhdPOukc01pZkcY08Rr+ddWlqkadWEdn7Oa7OXLpGhdEJmdwcjNu4UTwWwEuf+Rl/hB/EVSwCCRIkSJHMcRW0uW7SXJw7FrZAIn1l5q2m+mugk6uwUvQO4wm3iLZ7nV0+IzVmdqYC5h7j2bgh0MGDIIIBVlPFSCCD316jh8UFG+gnTXD28VaF63/eWQQw4va3nzSSw/ZZuVGgpHnoNJmrqomqGiS2pYhVBZjuABJPgBqaLYXovjLvq2GUc3IT4MZ+FG/Rxs/tviWHqjIn2j67eQgeZr0UPUI2eWnoVdQTduKvcgZj7zlHzqhd2PaQ6u7eSr+deibUuFhrlgordmTGbUKTxOXKTG7NGu+sTjj2jROxKLi6fZQXB2h7BPi5/CKGYllk5QAJMATuGnHnBNEcQ8KY37h4nQUHuHUxuGg8BpVIjk0mojszZLXgWnKoMAxMnjFHcN0at6SGc95gfCKlFsxxp1tsdyk+ANbxdlW09W2o741951qli7cVaJZlBhn5R404wx4mil1arsKpG2VBhh31FiFAMDhv8atu2UFvd40PJoBqVKlQGh2Fhzn6w6BZgni5Uhde6cx8Bzq8hnwGnkNwFc3AAAokADdx5k+JOtd2LeoFeaX3Oz1L7VxK+JXMypuUSROmZjGdvko7lHfRfBdIGtYZ8IqGHcHOoOix21MbyTGvjVgWrVjHWOsUMtm2GZDqGuG29xQR9t18hUO28QblyQuVEGVBEab2aBoCzEsY01jcBWo1SOMk7YHFhQS0bzXVy8F1O7go0nz4Dmav2sGoAa40EgFU4heDvxVTwG9u4a1VxL2DoVZtd8ATHnW7snHXZWOKBQNml53AQFH7I/GhWIvEkneTxonjruH6sLbW6Hzas5Qplg6ADWZ48qCPWo9dEk/FjM9c08VIi1owRxTFatFKYrQpWC12jFSGUwQQQeRGorsrTRQGgsXxcTrF03B1+qx4gfVbWO8Ecps22rO4HFG2+YCQRlZTuZDvB5bhrwIB4UckDKyklHGZSd5G4gxoHB0I59xFQMJ2Gonh3oFau1etX6pDUpj8yhbmsCFbjHAHn+uFcLfoGuKqQYqolQC73qyvSfZAvDrLYi4Bu+uBwP7XI+XgSOKqJ8RNUHnYEHXSN4/OpRR7beADzcQdr2h9Yc/tfOgSrUOidnDWypBHDcflVg2tZG46jz4eRkVd2dhetBUeso07+IHzHuqQWCBBG4/8ANX8kW04/s5wgg0bs4oKKDRFcPerRzDGI2medU7G1GV5U6+8SN0jiOHnQq7dmoA1GixdM6xyBXOQQraqOQO9fI6eEVUyliABJJAA7zRC7/WL37/4uPkai2fAcMfZ3eJ0rB0euj0LZLrZtpbXcgieZOrHzJJoomM0iax1jF99XExtDIVxt5QsKAByGgrIY19TRPE4qQdaCYh6gbI1TO6rwAJPugfE06bJRmCjN4zuFd4ACGc8TA8Bv4c5opgVjXiaoaaVhjA4RVUKogAQO4UXTCwI4nf3Dl4nj5DnVPAvAB4+yP93lw7/CidtxVJ0VcQlvq2QJLsxDOw0RBEBJ3sde0NwMTWax6VqsU4is3tI/rv8A18qyo0blkbpeF0jPX1qqy1cv0Nx13KIG8/AVo5lPFXcx03DQfnVelSoUVKlSoDRrIYhgwYGCpkMPEHdRXB4q3aYXHU3MuoRTlBI17T8B4AnwrXdCtnpjGOHxtlXNpFa1cYdtkmGQ3F1cL2Y13aU3Szo+mHuOqW1S25DJAGg0JHPQyPCufG0dW6YE6MX0xm0WxGLZEQB71z2U7CqiKszp6ump7J51c6cdKLM9Xg7Kg8bzrJB4ZFOk95rL43Csp08YHzHOtBfwdm5h8NiVEEl0uroYu21lDHJgQaUT9gbHWlSLiOzi6iMS2rhwCro/MhgdeIKmguKvFiSdSd5ovi1jRdAKGNYJ1jSto5v8FFiTXDJRBcPrU7YOSYECdJ1IHCTxrRAR1ZqREoquBjhNO+F03UAJauWoicJzpNhRQoLBpyKvPho4VE1moCtlq7s7FZZtuewxmd+R4gP4Row4jvAqAW65ZKANZypKtvB/RB4g7weIINSpiKG4dyyhD66+ofrL9Tx4r5r9WuBdoQMjFU/0qg/XU4vUAW+ld9L6TQoXq6F6gCRv0Mx9n21Hew/ECuxcpdZQWcbKxQt3FcSNYJ89/kYNbDamzxIuIOy4nwJ0I981hMSmU5lOnERIHfB4VqNn7YL2OrMsymVhsqxuMEz7uc1QyIbPZ1lRpqNSBqDrVK/sth6zIvif5VcbCM5lnyjXsqxgzGp0idN/fSH0e1vILfaAPw8qWFFsE/0bO5832VLfKpLOx2aMoZgdx0Xu41oE6RKmlvC2ZgHM4Zt/L3bqgxHTHFAHLct2u5Lag+8g0sUDk2DePq2X75geepFVMfsm7am46ZV0kFkzTuPYDZo47ql2rta/dYZrj3DGrsZ03xA0io7eBcr2+I4d/hUs1xI7OJHOPHSrK4nvplwzLyPiD864eyONr3R/KoKO3vVSv3N9K+gB0Vx5kfCuMPZNxgBmgEE5uAHKhC5grZgchoO8+0ffNFsMOe4b/wAqhRAIA8BUoPwoVu9hOzfkz/x3Ad1XreIoGjxU64jvqmQliMTp+v1+hQLGXJqZ8RIk/oUOxD0K/Yp33ABJ3CgF64WYsePyq9tO9JyDhv8AGhxFANSpUqAVKlSoQ3FvaTAZQSNdOY5EEbj4VYxm2blxsz3GcyR2iSANIyydJis0l+OP/FSC/wB9SjVsOi+GEN/x4GpsLfyo9versrjudQyz3EqxHurOHEnnuqa1iqNWFKgqxU5pBPZ7P2sw1PdGb4VwlnvGoBIHyPeKq2700Tw+ok1SWNawk8KuJhByqawBzq9Z5Sdd/f4jjQA9cFvPhp+v1rSOEoosd1dOBwoAE+z54VE+zo4VW25grhuMyXnCnXKZIB4gRwoFfwdziyt45vxoA3iLCDeyjxYCqTm1xuJ/iFCEw0HtJpB9Uazw3tuqVbuUf3SEka5k3fZOYz4wKMlly41obnQ/xA/KoGdDuYHwBP4VJb2uyns2rQ7sixpxMg6jnRG10mvKhPVIQSpJHZJzSFzBGBO7lvEViUpLpfydFGPuDLWHzGBm7oRiZnSI1HjSx+HYdplZW3sGUpm1jrADwJ0PJvtCiDbetSS2EDNvnrHA03k5QSdQeIiPOosFtNb95LTW0RWzLmXrGIJBInO5DAmARAkcjrVcl2k9e44xur/gDZqfPWi2n0aSxbZ2vWyQAVtC5DkEjSGSQYO7XcfGgIuAHS2nmXc/AgfCkZqStElBp0zkPXaNO7Xw1qVMREEBAeYtJPvcEipjinP/AJLh7g5QR4LAq7M6I0sXDutv45WA95EUsvAsgPLOk+4Ga7yzvQH7Ut96a6BfgwQcl7I/y02BlwzHmfspcb45Y+Nd3dlXLDKQVKsiPEwcraiOR7qbLOrOT+uZqyMSsRJPiZqbsqIMQj3BlOYCeAGvnm8ajTZf6J/ACrRxQpfTBVNOVjnZoYyzE9wMAdwHKprWzbQ9geevzqD6cOddDHDnQF5cOo3ADwFdZBVD6d30/wBOFAXSgpjbFVBjRXQxg50ITvaESYgc+VVFIk5Y8iD+jUj4pSCDuIg+BoLdwsHsHThrrQywz1yjfpHcaX0hPrDzMfOgyMy/WB5iZ+FSnGXOLsT+0J+LA1dCgp1o5j3ikXmhYx9waq5kcgv5VGMa4O5T4oh+YoHQWe7VW5maQgLNBgD51SfHM3sr4ouX5aR5VYwmORRDqxJ0zKRMbxOaR7gKBJPsH3Nn3Rqbb+OVj8hVV8O43ow8VI/Cj/8AS49lT5jw4gjnSO0wd6P5PlHuMmjRfBm8h5Gua0H07ms6aS34RXX09Vjs2zzzDd+dBRnKVH7+MsTIRG1M/wBXl0nQ6Gq/0yz/APSvuP50MkcwaZjSpULLsYNXaN86VKhC3ZumiuHvGKVKozQQs3yZO7SdPKpkxTUqVVdg7OKaK6TFMRSpUIN15O+oHg8BSpUBF1S/VFcXsGhHqgUqVCFF8CnKqV7DKBu86VKhSndw4qDD29RBgzvHDXeOVNSqAtXLEsWLEniSSSSd5magiKVKpHosuxxd7hXQxLU9KtEOTiWqNsS1KlUKRHEtXP0g0qVAP1xpNeNKlQDdcaXXGlSoEP1xpdcaelQ0N9JanGJalSoZOhiWpfSWpUqA7W+ac3jSpUBH1s7wK56z9SaVKgG6066/L8qQZo9Y0qVUjGzHmajNw8z7zSpUIMaRampUAxuGmznnSpUB/9k=',
    }


const options = [
    {
        value: 'Option1',
        label: 'Mặc định',
    },
    {
        value: 'Option2',
        label: 'Tên A-Z',
    },
    {
        value: 'Option3',
        label: 'Tên Z-A',
    },
    {
        value: 'Option4',
        label: 'Giá giảm dần',
    },
    {
        value: 'Option5',
        label: 'Giá tăng dần',
    },
]

const handleClick = (item) => {
    router.push(item && item.id ? `/product/${item.id}` : '')
}

const handleChangePage = (value) => {
    products.value = []
    getProducts()
}

getProducts()
</script>

<template>
    <div>
        <div v-loading="loading" class="w-4/5 flex flex-col items-center justify-center  mx-auto">
            <CategoryProducts>
                <img :src="advertising && advertising.img1 ? advertising.img1 : ''"
                     alt="">
                <div class="flex items-center justify-center h-10 bg-red-600 my-4 text-white font-semibold">
                    HELLO SHOP - PHÂN PHỐI CHÍNH HÃNG CÁC SẢN PHẨM LAPTOP
                </div>
                <img :src="advertising && advertising.img2 ? advertising.img2 : ''" alt="">
<!--                <div class="flex h-16 items-center bg-gray-500 px-8">-->
<!--                    <div>-->
<!--                        <span class="text-white">-->
<!--                        Sắp xếp theo:-->
<!--                        </span>-->
<!--                        <el-select-->
<!--                            v-model="value"-->
<!--                            class="m-2"-->
<!--                            default-first-option-->
<!--                            size="small"-->
<!--                            style="width: 120px;"-->
<!--                        >-->
<!--                            <el-option-->
<!--                                v-for="item in options"-->
<!--                                :key="item.value"-->
<!--                                :label="item.label"-->
<!--                                :value="item.value"-->
<!--                            />-->
<!--                        </el-select>-->
<!--                    </div>-->
<!--                </div>-->
                <div class="w-full grid grid-cols-4 gap-4">
                    <div v-for="(product, index) in products"
                         :key="index"
                         class="mt-4 hover:mt-1 card-new-product">
                        <div class="w-full bg-white border border-solid border-gray-300 rounded-xl overflow-hidden cursor-pointer" @click="handleClick(product)">
                            <div class="w-full relative">
                                <img class="w-full h-full" :src="product.img" alt="">
                                <div class="w-full py-2 text-center text-white bg-red-500 uppercase font-semibold absolute bottom-0">
                                    {{ product.status }}
                                </div>
                            </div>
                            <div class=" h-[5rem] text-sm p-3 text-center font-semibold hover:text-blue-800 ">
                                {{ product.name }}
                            </div>
                            <div class="p-2 text-center border-solid border-t-gray-200 border border-b-blue-300 border-b-2 text-red-600 font-semibold">
                                {{ product.price }}đ
                            </div>
                            <div class="py-2 hover text-center font-bold text-blue-600 uppercase flex justify-center items-center gap-1 hover:text-white hover:bg-red-600 transition duration-200">
                                <el-icon size="20"> <ShoppingCart/> </el-icon>
                                Thêm vào giỏ
                            </div>
                        </div>
                    </div>
                </div>
                <el-pagination
                    v-model:current-page="formState.page"
                    :page-size="formState.size"
                    layout="prev, pager, next"
                    :total="formState.total"
                    @currentChange="handleChangePage"
                />
            </CategoryProducts>
        </div>
    </div>
</template>

<style lang="scss">

</style>