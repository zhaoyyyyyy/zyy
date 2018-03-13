import cn.edu.hfut.dmic.webcollector.model.CrawlDatums;
import cn.edu.hfut.dmic.webcollector.model.Page;
import cn.edu.hfut.dmic.webcollector.plugin.berkeley.BreadthCrawler;
import org.jsoup.nodes.Document;

public class NewsCrawler extends BreadthCrawler {
    public NewsCrawler(String crawlPath, boolean autoParse) {
        super(crawlPath, autoParse);
        this.addSeed("http://news.hfut.edu.cn/list-1-1.html");
        this.addRegex("http://news.hfut.edu.cn/show-.*html");
        this.addRegex("-.*\\.(jpg|png|gif).*");
        this.addRegex("-.*#.*");

        setThreads(50);
        getConf().setTopN(100);
    }

    @Override
    public void visit(Page page, CrawlDatums next) {
        String url = page.url();
        if (page.matchUrl("http://news.hfut.edu.cn/show-.*html")) {
            String title = page.select("div[id=Article]>h2").first().text();
            String content = page.selectText("div#artibody");

            System.out.println("URL:\n" + url);
            System.out.println("title:\n" + title);
            System.out.println("content:\n" + content);
        }
    }

    public static void main(String[] args) throws Exception {
        NewsCrawler crawler = new NewsCrawler("crawl", true);
        crawler.start(4);
    }

}
