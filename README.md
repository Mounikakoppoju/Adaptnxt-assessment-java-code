# Adaptnxt-assessment-java-code
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.FileWriter;
import java.io.IOException;
import com.opencsv.CSVWriter;

public class ProductScraper {

    public static void main(String[] args) throws IOException {
        String url = "https://www.staples.com/Computer-office-desks/cat_CL210795/663ea?icid=BTS:2020:STUDYSPACE:DESKS";
        Document doc = Jsoup.connect(url).get();
        Elements products = doc.select(".product");
        CSVWriter writer = new CSVWriter(new FileWriter("products.csv"));
        String[] header = {"Product Name", "Product Price", "Item Number", "Model Number", "Product Category", "Product Description"};
        writer.writeNext(header);

        for (int i = 0; i < 10 && i < products.size(); i++) {
            Element product = products.get(i);
            String name = product.select(".product-title a").text();
            String price = product.select(".product-price").text();
            String itemNumber = product.select(".product-sku").text();
            String modelNumber = product.select(".product-model-number").text();
            String category = product.select(".breadcrumb-item:last-child").text();
            String description = product.select(".product-description").text();

            String[] data = {name, price, itemNumber, modelNumber, category, description};
            writer.writeNext(data);
        }

        writer.close();
    }
}
